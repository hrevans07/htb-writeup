## Nmap
```bash
└─$ sudo portscan 10.10.10.229
[sudo] password for hunter: 
Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-17 15:35 CDT
Nmap scan report for 10.10.10.229
Host is up (0.047s latency).

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.1 (protocol 2.0)
| ssh-hostkey: 
|_  4096 52:47:de:5c:37:4f:29:0e:8e:1d:88:6e:f9:23:4d:5a (RSA)
80/tcp   open  http    nginx 1.17.4
|_http-server-header: nginx/1.17.4
|_http-title: Site doesn't have a title (text/html).
3306/tcp open  mysql   MySQL (unauthorized)
|_ssl-cert: ERROR: Script execution failed (use -d to debug)
|_ssl-date: ERROR: Script execution failed (use -d to debug)
|_sslv2: ERROR: Script execution failed (use -d to debug)
|_tls-alpn: ERROR: Script execution failed (use -d to debug)
|_tls-nextprotoneg: ERROR: Script execution failed (use -d to debug)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 39.57 seconds
```
* Visit the web server to see a webpage with two links:
	* spectra.htb/testing/index.php
	* spectra.htb/main/index.php

## Testing
![[Pasted image 20210617154121.png]]
* Get a page saying there's a database error
* This most likely refers to the port 3306 mysql that we saw in the nmap

## Main
![[Pasted image 20210617154232.png]]
* Get a wordpress page
* Possible username of administrator
```php
<meta name\="generator" content\="WordPress 5.4.2" /\>
```
* The source for the index page leaks the version
* Look into php deserialization --> phpggc