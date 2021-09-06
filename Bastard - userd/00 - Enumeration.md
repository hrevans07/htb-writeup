## Initial Enumeration
* Robots.txt leaks files?
* Web server is running Drupal 7
* Microsoft IIS 7.5 -> Windows 7 | Windows Server 2008 R2
```bash
# Nmap 7.91 scan initiated Sat May 22 12:07:15 2021 as: nmap -sC -sV -oA nmap/init 10.10.10.9
Nmap scan report for 10.10.10.9
Host is up (0.054s latency).
Not shown: 997 filtered ports
PORT      STATE SERVICE VERSION
80/tcp    open  http    Microsoft IIS httpd 7.5
|_http-generator: Drupal 7 (http://drupal.org)
| http-methods: 
|_  Potentially risky methods: TRACE
| http-robots.txt: 36 disallowed entries (15 shown)
| /includes/ /misc/ /modules/ /profiles/ /scripts/ 
| /themes/ /CHANGELOG.txt /cron.php /INSTALL.mysql.txt 
| /INSTALL.pgsql.txt /INSTALL.sqlite.txt /install.php /INSTALL.txt 
|_/LICENSE.txt /MAINTAINERS.txt
|_http-server-header: Microsoft-IIS/7.5
|_http-title: Welcome to 10.10.10.9 | 10.10.10.9
135/tcp   open  msrpc   Microsoft Windows RPC
49154/tcp open  msrpc   Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat May 22 12:08:24 2021 -- 1 IP address (1 host up) scanned in 69.72 seconds
```