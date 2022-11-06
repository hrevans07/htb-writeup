## Nmap
```bash
# Nmap 7.92 scan initiated Wed Aug 24 19:34:00 2022 as: nmap -sC -sV -oA nmap/init 10.10.11.166
Nmap scan report for 10.10.11.166
Host is up (0.044s latency).
Not shown: 996 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 61:ff:29:3b:36:bd:9d:ac:fb:de:1f:56:88:4c:ae:2d (RSA)
|   256 9e:cd:f2:40:61:96:ea:21:a6:ce:26:02:af:75:9a:78 (ECDSA)
|_  256 72:93:f9:11:58:de:34:ad:12:b5:4b:4a:73:64:b9:70 (ED25519)
25/tcp open  smtp    Postfix smtpd
|_smtp-commands: debian.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, CHUNKING
53/tcp open  domain  ISC BIND 9.11.5-P4-5.1+deb10u7 (Debian Linux)
| dns-nsid: 
|_  bind.version: 9.11.5-P4-5.1+deb10u7-Debian
80/tcp open  http    nginx 1.14.2
|_http-title: Coming Soon - Start Bootstrap Theme
|_http-server-header: nginx/1.14.2
Service Info: Host:  debian.localdomain; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Aug 24 19:34:49 2022 -- 1 IP address (1 host up) scanned in 49.33 seconds
```
### Port 25 - SMTP
- Some sort of mail server judging by its banner
- Not much to do there if there's no CVE for the version...Which I don't know
### Port 53 - DNS
Can do a couple of things. In this case a zone transfer gives some interesting items.
```bash
$ dig axfr @10.10.11.166 trick.htb                                                                                                                  130 ⨯

; <<>> DiG 9.18.4-2-Debian <<>> axfr @10.10.11.166 trick.htb
; (1 server found)
;; global options: +cmd
trick.htb.              604800  IN      SOA     trick.htb. root.trick.htb. 5 604800 86400 2419200 604800
trick.htb.              604800  IN      NS      trick.htb.
trick.htb.              604800  IN      A       127.0.0.1
trick.htb.              604800  IN      AAAA    ::1
preprod-payroll.trick.htb. 604800 IN    CNAME   trick.htb.
trick.htb.              604800  IN      SOA     trick.htb. root.trick.htb. 5 604800 86400 2419200 604800
;; Query time: 36 msec
;; SERVER: 10.10.11.166#53(10.10.11.166) (TCP)
;; WHEN: Wed Aug 24 20:10:28 CDT 2022
;; XFR size: 6 records (messages 1, bytes 231)
```
3 domain names:
- trick.htb
- root.trick.htb
- preprod-payroll.trick.htb
The only obviously interesting one is the last of the 3.
### Port 80 - Web server
The landing page for browsing to the IP as well as the first 2 domains listed above are all the same.
![[Pasted image 20220824201239.png]]
Head over to the last of the domains to find something different.
![[Pasted image 20220824201336.png]]
Note that this web app is PHP
### Third subdomain
There is a third subdomain that is more difficult to find than the other two: ```preprod-marketing.trick.htb```
This one must be fuzzed once the naming scheme used in ```preprod-payroll.trick.htb``` is identified.
![[Pasted image 20220906162041.png]]