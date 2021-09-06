## Initial Nmap
```bash
sudo nmap -sC -sV -oA nmap/init 10.10.10.17
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-23 12:55 CDT
Nmap scan report for 10.10.10.17
Host is up (0.051s latency).
Not shown: 995 filtered ports
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 7.2p2 Ubuntu 4ubuntu2.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 94:d0:b3:34:e9:a5:37:c5:ac:b9:80:df:2a:54:a5:f0 (RSA)
|   256 6b:d5:dc:15:3a:66:7a:f4:19:91:5d:73:85:b2:4c:b2 (ECDSA)
|_  256 23:f5:a3:33:33:9d:76:d5:f2:ea:69:71:e3:4e:8e:02 (ED25519)
25/tcp  open  smtp     Postfix smtpd
|_smtp-commands: brainfuck, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, 
110/tcp open  pop3     Dovecot pop3d
|_pop3-capabilities: PIPELINING SASL(PLAIN) CAPA UIDL AUTH-RESP-CODE TOP USER RESP-CODES
143/tcp open  imap     Dovecot imapd
|_imap-capabilities: LOGIN-REFERRALS ID IMAP4rev1 IDLE LITERAL+ more have ENABLE Pre-login capabilities SASL-IR AUTH=PLAINA0001 post-login listed OK
443/tcp open  ssl/http nginx 1.10.0 (Ubuntu)
|_http-server-header: nginx/1.10.0 (Ubuntu)
|_http-title: Welcome to nginx!
| ssl-cert: Subject: commonName=brainfuck.htb/organizationName=Brainfuck Ltd./stateOrProvinceName=Attica/countryName=GR
| Subject Alternative Name: DNS:www.brainfuck.htb, DNS:sup3rs3cr3t.brainfuck.htb
| Not valid before: 2017-04-13T11:19:29
|_Not valid after:  2027-04-11T11:19:29
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
| tls-nextprotoneg: 
|_  http/1.1
Service Info: Host:  brainfuck; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 56.25 seconds
```
* Running Ubuntu Linux Xenial
* email related stuff
	* smtp is open
	* pop3 is open
	* imap is open
* Web server running on port 443 -> https
	* brainfuck.htb
	* DNS: sup3rs3cr3t.brainfuck.htb
	* hostname: brainfuck
	* Email leaked in certificate: orestis@brainfuck.htb
	* Visiting webpage just shows default nginx page
![[Pasted image 20210523130917.png]]