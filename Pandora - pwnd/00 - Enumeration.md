## Nmap
```bash
# Nmap 7.91 scan initiated Thu Jan 27 13:36:19 2022 as: nmap -sC -sV -oA nmap/init 10.10.11.136
Nmap scan report for 10.10.11.136
Host is up (0.031s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 24:c2:95:a5:c3:0b:3f:f3:17:3c:68:d7:af:2b:53:38 (RSA)
|   256 b1:41:77:99:46:9a:6c:5d:d2:98:2f:c0:32:9a:ce:03 (ECDSA)
|_  256 e7:36:43:3b:a9:47:8a:19:01:58:b2:bc:89:f6:51:08 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Play | Landing
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Jan 27 13:36:28 2022 -- 1 IP address (1 host up) scanned in 8.83 seconds
```
- Nothing obvious on the web server, trying a UDP scan shows that the server is running snmp
- Try and snmp walk and get some creds for user daniel
```bash
iso.3.6.1.2.1.25.4.2.1.5.1014 = STRING: "-k start"                                                                                                                                                                                                                                                                           
iso.3.6.1.2.1.25.4.2.1.5.1015 = STRING: "-c sleep 30; /bin/bash -c '/usr/bin/host_check -u daniel -p HotelBabylon23'"                                                                                                                                                                                                        
iso.3.6.1.2.1.25.4.2.1.5.1020 = STRING: "-k start"                                                                                                                                                                                                                                                                           
iso.3.6.1.2.1.25.4.2.1.5.1097 = STRING: "-u daniel -p HotelBabylon23
```