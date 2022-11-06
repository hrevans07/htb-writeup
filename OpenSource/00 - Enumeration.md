## Nmap
```bash
# Nmap 7.92 scan initiated Tue May 24 13:25:19 2022 as: nmap -sC -sV -oA nmap/init 10.129.180.202
Nmap scan report for 10.129.180.202
Host is up (0.044s latency).
Not shown: 997 closed tcp ports (reset)
PORT     STATE    SERVICE VERSION
22/tcp   open     ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 1e:59:05:7c:a9:58:c9:23:90:0f:75:23:82:3d:05:5f (RSA)
|   256 48:a8:53:e7:e0:08:aa:1d:96:86:52:bb:88:56:a0:b7 (ECDSA)
|_  256 02:1f:97:9e:3c:8e:7a:1c:7c:af:9d:5a:25:4b:b8:c8 (ED25519)
80/tcp   open     http    Werkzeug/2.1.2 Python/3.10.3
|_http-server-header: Werkzeug/2.1.2 Python/3.10.3
| fingerprint-strings: 
|   GetRequest: 
|     [TRUNCATED]
|_http-title: upcloud - Upload files for Free!
3000/tcp filtered ppp
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
[TRUNCATED]
```
### Web server
Visit port 80 to see a web server that has a couple interesting pages: /download and /upcloud
![[Pasted image 20220909140144.png]]
- /upcloud is some sort of file sharing site
- /download appears to be the source code for upcloud