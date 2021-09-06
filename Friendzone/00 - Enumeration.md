# Nmap
```bash
# Nmap 7.91 scan initiated Sat Jul 10 11:02:30 2021 as: nmap -p 21,22,53,80,139,443,445, -sC -sV -oA nmap/final 10.10.10.123
Nmap scan report for 10.10.10.123
Host is up (0.046s latency).

PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 3.0.3
22/tcp  open  ssh         OpenSSH 7.6p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 a9:68:24:bc:97:1f:1e:54:a5:80:45:e7:4c:d9:aa:a0 (RSA)
|   256 e5:44:01:46:ee:7a:bb:7c:e9:1a:cb:14:99:9e:2b:8e (ECDSA)
|_  256 00:4e:1a:4f:33:e8:a0:de:86:a6:e4:2a:5f:84:61:2b (ED25519)
53/tcp  open  domain      ISC BIND 9.11.3-1ubuntu1.2 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.11.3-1ubuntu1.2-Ubuntu
80/tcp  open  http        Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Friend Zone Escape software
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
443/tcp open  ssl/http    Apache httpd 2.4.29
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: 404 Not Found
| ssl-cert: Subject: commonName=friendzone.red/organizationName=CODERED/stateOrProvinceName=CODERED/countryName=JO
| Not valid before: 2018-10-05T21:02:30
|_Not valid after:  2018-11-04T21:02:30
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
445/tcp open  netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)
Service Info: Hosts: FRIENDZONE, 127.0.1.1; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: -47m53s, deviation: 1h43m55s, median: 12m06s
|_nbstat: NetBIOS name: FRIENDZONE, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)
|   Computer name: friendzone
|   NetBIOS computer name: FRIENDZONE\x00
|   Domain name: \x00
|   FQDN: friendzone
|_  System time: 2021-07-10T19:14:51+03:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-07-10T16:14:51
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Jul 10 11:02:53 2021 -- 1 IP address (1 host up) scanned in 23.12 seconds
```
## ftp
Does not allow anonymous login
## DNS
A zone transfer leaks some subdomains of the box
- administrator1.friendzone.red
- hr.friendzone.red
- uploads.friendzone.red
```bash
hunter@kali ~/htb/friendzone$ dig axfr @10.10.10.123 friendzone.red
; <<>> DiG 9.16.15-Debian <<>> axfr @10.10.10.123 friendzone.red
; (1 server found)
;; global options: +cmd
friendzone.red.         604800  IN      SOA     localhost. root.localhost. 2 604800 86400 2419200 604800
friendzone.red.         604800  IN      AAAA    ::1
friendzone.red.         604800  IN      NS      localhost.
friendzone.red.         604800  IN      A       127.0.0.1
administrator1.friendzone.red. 604800 IN A      127.0.0.1
hr.friendzone.red.      604800  IN      A       127.0.0.1
uploads.friendzone.red. 604800  IN      A       127.0.0.1
friendzone.red.         604800  IN      SOA     localhost. root.localhost. 2 604800 86400 2419200 604800
;; Query time: 52 msec
;; SERVER: 10.10.10.123#53(10.10.10.123)
;; WHEN: Sat Jul 10 11:15:45 CDT 2021
;; XFR size: 8 records (messages 1, bytes 289)
```
## Port 80 - Web server
Turns up same webpage w/ no links or anything else very interesting for every subdomain we try

Potentially leaks email address
```email
info@friendzoneportal.red
```

Doing a gobuster reveals there is a /wordpress/ directory on this server
- Not an actual wordpress site, just an empy directory
## Port 443 - Web server
Certificate leaks email address
```email
haha@friendzone.red
```
### ==administrator1.friendzone.htb==
- *FriendZone Corp Administrator login page*
- login posts to /login.php

### ==uploads.friendzone.htb==

- Some kind of page for uploading files
- uploading a file *test.php* yields this response

![[Pasted image 20210710114022.png]]
## SMB
Know from Nmap that web servers OS is linux, but there are smb shares on the box
- Some type of virtualization happening here?

```bash
hunter@kali ~/htb/friendzone$ smbclient -L \\10.10.10.123
Enter WORKGROUP\hunter's password: 

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        Files           Disk      FriendZone Samba Server Files /etc/Files
        general         Disk      FriendZone Samba Server Files
        Development     Disk      FriendZone Samba Server Files
        IPC$            IPC       IPC Service (FriendZone server (Samba, Ubuntu))
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        WORKGROUP            HARIS-PC
```
The 'general' and 'Development' shares support anonymous login
- Find *creds.txt* in the 'general' share

```bash
hunter@kali ~/htb/friendzone$ cat creds.txt
creds for the admin THING:

admin:WORKWORKHhallelujah@#
```