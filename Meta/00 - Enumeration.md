## Nmap
```bash
# Nmap 7.92 scan initiated Thu May  5 13:42:19 2022 as: nmap -sC -sV -oA nmap/init 10.10.11.140
Nmap scan report for 10.10.11.140
Host is up (0.041s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 12:81:17:5a:5a:c9:c6:00:db:f0:ed:93:64:fd:1e:08 (RSA)
|   256 b5:e5:59:53:00:18:96:a6:f8:42:d8:c7:fb:13:20:49 (ECDSA)
|_  256 05:e9:df:71:b5:9f:25:03:6b:d0:46:8d:05:45:44:20 (ED25519)
80/tcp open  http    Apache httpd
|_http-title: Did not follow redirect to http://artcorp.htb
|_http-server-header: Apache
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu May  5 13:42:29 2022 -- 1 IP address (1 host up) scanned in 9.15 seconds
```
- Go to web server and see that it's a static site w/ nothing really interesting
- See the hostname leaked in Nmap output -> decide to try a subdomain bruteforce
```bash
gobuster vhost -u artcorp.htb -w /opt/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:          http://artcorp.htb
[+] Method:       GET
[+] Threads:      10
[+] Wordlist:     /opt/SecLists/Discovery/DNS/subdomains-top1million-5000.txt 
[+] User Agent:   gobuster/3.1.0
[+] Timeout:      10s
===============================================================
2022/05/05 13:43:41 Starting gobuster in VHOST enumeration mode
===============================================================
Found: dev01.artcorp.htb (Status: 200) [Size: 247]
===============================================================
2022/05/05 13:44:01 Finished
===============================================================
```
- Found subdomain dev01.artcorp.htb