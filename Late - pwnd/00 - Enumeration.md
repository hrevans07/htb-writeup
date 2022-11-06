## Nmap
```bash
# Nmap 7.92 scan initiated Tue Apr 26 12:16:56 2022 as: nmap -sC -sV -oA nmap/init 10.129.168.94
Nmap scan report for 10.129.168.94
Host is up (0.10s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 02:5e:29:0e:a3:af:4e:72:9d:a4:fe:0d:cb:5d:83:07 (RSA)
|   256 41:e1:fe:03:a5:c7:97:c4:d5:16:77:f3:41:0c:e9:fb (ECDSA)
|_  256 28:39:46:98:17:1e:46:1a:1e:a1:ab:3b:9a:57:70:48 (ED25519)
80/tcp open  http    nginx 1.14.0 (Ubuntu)
|_http-title: Late - Best online image tools
|_http-server-header: nginx/1.14.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Apr 26 12:17:12 2022 -- 1 IP address (1 host up) scanned in 15.81 seconds
```
- Web app is static and only reveals the hostname and a subdomain
![[Pasted image 20220509120815.png]]
- Check out subdomain and see it's an app for converting images to text
![[Pasted image 20220509120903.png]]