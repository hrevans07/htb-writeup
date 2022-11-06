## Nmap
```bash
# Nmap 7.92 scan initiated Tue Oct 18 13:59:10 2022 as: nmap -sC -sV -oA nmap/init 10.10.11.183
Nmap scan report for 10.10.11.183
Host is up (0.038s latency).
Not shown: 996 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 29:dd:8e:d7:17:1e:8e:30:90:87:3c:c6:51:00:7c:75 (RSA)
|   256 80:a4:c5:2e:9a:b1:ec:da:27:64:39:a4:08:97:3b:ef (ECDSA)
|_  256 f5:90:ba:7d:ed:55:cb:70:07:f2:bb:c8:91:93:1b:f6 (ED25519)
80/tcp   open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-generator: Hugo 0.94.2
|_http-title: Ambassador Development Server
|_http-server-header: Apache/2.4.41 (Ubuntu)
3000/tcp open  ppp?
| fingerprint-strings: 
|   [TRUNCATED]
3306/tcp open  mysql   MySQL 8.0.30-0ubuntu0.20.04.2
| mysql-info: 
|   Protocol: 10
|   Version: 8.0.30-0ubuntu0.20.04.2
|   Thread ID: 9
|   Capabilities flags: 65535
|   Some Capabilities: ConnectWithDatabase, Support41Auth, SupportsTransactions, SupportsCompression, Speaks41ProtocolOld, IgnoreSigpipes, IgnoreSpaceBeforeParenthesis, FoundRows, SwitchToSSLAfterHandshake, ODBCClient, Speaks41ProtocolNew, LongPassword, DontAllowDatabaseTableColumn, LongColumnFlag, InteractiveClient, SupportsLoadDataLocal, SupportsAuthPlugins, SupportsMultipleStatments, SupportsMultipleResults
|   Status: Autocommit
|   Salt: Q?VQhu\x1A-\x1CFSI4*\x1A`=(mq
|_  Auth Plugin Name: caching_sha2_password
|_sslv2: ERROR: Script execution failed (use -d to debug)
|_tls-nextprotoneg: ERROR: Script execution failed (use -d to debug)
|_ssl-cert: ERROR: Script execution failed (use -d to debug)
|_ssl-date: ERROR: Script execution failed (use -d to debug)
|_tls-alpn: ERROR: Script execution failed (use -d to debug)
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
[TRUNCATED]
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Oct 18 14:01:09 2022 -- 1 IP address (1 host up) scanned in 119.37 seconds
```
### Port 80 web server: 
![[Pasted image 20221025184419.png]]
Static site running hugo...not much going on here.
### Port 3000 web server: 
![[Pasted image 20221025184552.png]]
Note the version is leaked near the bottom of the page