## Nmap
```bash
# Nmap 7.92 scan initiated Sat Oct 22 18:23:14 2022 as: nmap -sC -sV -oA nmap/init 10.10.11.177
Nmap scan report for 10.10.11.177
Host is up (0.039s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 9e:1f:98:d7:c8:ba:61:db:f1:49:66:9d:70:17:02:e7 (RSA)
|   256 c2:1c:fe:11:52:e3:d7:e5:f7:59:18:6b:68:45:3f:62 (ECDSA)
|_  256 5f:6e:12:67:0a:66:e8:e2:b7:61:be:c4:14:3a:d3:8e (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Is my Website up ?
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Oct 22 18:23:24 2022 -- 1 IP address (1 host up) scanned in 9.26 seconds
```
Check out the web app to see it is intended to check if a provided web site is up or down. A host name is also leaked near the bottom of the page.
![[Pasted image 20221022215052.png]]
Do some directory fuzzing and find ```/dev/.git```
Do a virtual host brute force to leak a subdomain:
```bash
$ gobuster vhost -u http://siteisup.htb -w /opt/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:          http://siteisup.htb
[+] Method:       GET
[+] Threads:      10
[+] Wordlist:     /opt/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt
[+] User Agent:   gobuster/3.1.0
[+] Timeout:      10s
===============================================================
2022/10/22 21:54:20 Starting gobuster in VHOST enumeration mode
===============================================================
Found: dev.siteisup.htb (Status: 403) [Size: 281]
```
The subdomain gives nothing but 403 though...