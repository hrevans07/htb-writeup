## Nmap
```bash
# Nmap 7.92 scan initiated Fri Apr 29 11:41:06 2022 as: nmap -sC -sV -oA nmap/init 10.10.11.154
Nmap scan report for 10.10.11.154
Host is up (0.041s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5 (protocol 2.0)
| ssh-hostkey: 
|   3072 77:b2:16:57:c2:3c:10:bf:20:f1:62:76:ea:81:e4:69 (RSA)
|   256 cb:09:2a:1b:b9:b9:65:75:94:9d:dd:ba:11:28:5b:d2 (ECDSA)
|_  256 0d:40:f0:f5:a8:4b:63:29:ae:08:a1:66:c1:26:cd:6b (ED25519)
80/tcp open  http    nginx
| http-title: Agency - Start Bootstrap Theme
|_Requested resource was /index.php?page=default.html
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Apr 29 11:41:16 2022 -- 1 IP address (1 host up) scanned in 9.31 seconds
```
Go to web app and immediatly notice it is using a GET parameter to fetch content
![[Pasted image 20220429121855.png]]
Confirm this with standard directory traversal payload
- Check out /etc/hosts for some weird stuff
![[Pasted image 20220429121938.png]]
- The rules for the LFI:
```php
<?php
function sanitize_input($param) {
    $param1 = str_replace("../","",$param);
    $param2 = str_replace("./","",$param1);
    return $param2;
}

$page = $_GET['page'];
if (isset($page) && preg_match("/^[a-z]/", $page)) {
    $page = sanitize_input($page);
} else {
    header('Location: /index.php?page=default.html');
}

readfile($page);
?>
```
- Run a dirbust on the LFI but checking for html files other than default.html shows a file:
```bash
─$ wfuzz --hl 0 -w /opt/SecLists/Discovery/Web-Content/raft-small-words.txt http://10.10.11.154/index.php?page=FUZZ.html
[TRUNCATED]
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://10.10.11.154/index.php?page=FUZZ.html
Total requests: 43003

=====================================================================
ID           Response   Lines    Word       Chars       Payload                                                                                    
=====================================================================

000000234:   200        188 L    824 W      11414 Ch    "default"                                                                                  
000000361:   200        72 L     304 W      4144 Ch     "beta"
```
- Go to this page and see a file upload that posts the uploaded file to /activate_license.php
- Grab this file with LFI
```php
<?php
if(isset($_FILES['licensefile'])) {
    $license      = file_get_contents($_FILES['licensefile']['tmp_name']);
    $license_size = $_FILES['licensefile']['size'];

    $socket = socket_create(AF_INET, SOCK_STREAM, SOL_TCP);
    if (!$socket) { echo "error socket_create()\n"; }

    if (!socket_connect($socket, '127.0.0.1', 1337)) {
        echo "error socket_connect()" . socket_strerror(socket_last_error()) . "\n";
    }

    socket_write($socket, pack("N", $license_size));
    socket_write($socket, $license);

    socket_shutdown($socket);
    socket_close($socket);
}
?>
```