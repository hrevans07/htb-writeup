## Nmap 
* 3 web servers
	* Ports 80/5985 -> HTTP
	* Port 443 -> SSL
```bash
# Nmap 7.91 scan initiated Sun Jun 27 10:13:19 2021 as: nmap -p 80,135,443,445,5985,6379,7680, -sC -sV -oA nmap/final 10.10.10.237
Nmap scan report for 10.10.10.237
Host is up (0.050s latency).

PORT     STATE SERVICE      VERSION
80/tcp   open  http         Apache httpd 2.4.46 ((Win64) OpenSSL/1.1.1j PHP/7.3.27)
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.46 (Win64) OpenSSL/1.1.1j PHP/7.3.27
|_http-title: Heed Solutions
135/tcp  open  msrpc        Microsoft Windows RPC
443/tcp  open  ssl/http     Apache httpd 2.4.46 ((Win64) OpenSSL/1.1.1j PHP/7.3.27)
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.46 (Win64) OpenSSL/1.1.1j PHP/7.3.27
|_http-title: Heed Solutions
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2009-11-10T23:48:47
|_Not valid after:  2019-11-08T23:48:47
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
445/tcp  open  microsoft-ds Windows 10 Pro 19042 microsoft-ds (workgroup: WORKGROUP)
5985/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
6379/tcp open  redis        Redis key-value store
7680/tcp open  pando-pub?
Service Info: Host: ATOM; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 2h31m26s, deviation: 4h02m32s, median: 11m24s
| smb-os-discovery: 
|   OS: Windows 10 Pro 19042 (Windows 10 Pro 6.3)
|   OS CPE: cpe:/o:microsoft:windows_10::-
|   Computer name: ATOM
|   NetBIOS computer name: ATOM\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2021-06-27T08:25:34-07:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-06-27T15:25:31
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Jun 27 10:14:44 2021 -- 1 IP address (1 host up) scanned in 85.39 seconds
```

## SMB
* Allows anonymous login for the Software_Updates disk
```bash
hunter@kali ~/htb/atom$ smbclient -L \\\\10.10.10.237
Enter WORKGROUP\hunter's password: 

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        Software_Updates Disk      
SMB1 disabled -- no workgroup available
hunter@kali ~/htb/atom$
```
* The client\* directories are empty
* The UAT_Testing_Procedures.pdf details quality assurance steps for some software that this company publishes
```bash
hunter@kali ~/htb/atom$ smbclient \\\\10.10.10.237\\Software_Updates
Enter WORKGROUP\hunter's password: 
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Sun Jun 27 10:37:52 2021
  ..                                  D        0  Sun Jun 27 10:37:52 2021
  client1                             D        0  Sun Jun 27 10:37:52 2021
  client2                             D        0  Sun Jun 27 10:37:52 2021
  client3                             D        0  Sun Jun 27 10:37:52 2021
  UAT_Testing_Procedures.pdf          A    35202  Fri Apr  9 06:18:08 2021

             4413951 blocks of size 4096. 1368389 blocks available
smb: \>
```

## Web Servers
* Port 80 and 443 servers are identical
* Port 5985 server returned a 404