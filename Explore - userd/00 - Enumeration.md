## Nmap
Whole lot of weird ports
* SSH running on port 2222
* Port 5555 is filtered but still shows up (?)
* Nmap runs http scripts agains port 39647 but nothing turns up
* Port 59777 running httpd for minecraft server
```bash
# Nmap 7.91 scan initiated Wed Jun 30 19:30:49 2021 as: nmap -p 2222,5555,39647,59777, -sC -sV -oA nmap/final 10.10.10.247
Nmap scan report for 10.10.10.247
Host is up (0.051s latency).

PORT      STATE    SERVICE VERSION
2222/tcp  open     ssh     (protocol 2.0)
| fingerprint-strings: 
|   NULL: 
|_    SSH-2.0-SSH Server - Banana Studio
| ssh-hostkey: 
|_  2048 71:90:e3:a7:c9:5d:83:66:34:88:3d:eb:b4:c7:88:fb (RSA)
5555/tcp  filtered freeciv
39647/tcp open     unknown
| fingerprint-strings: 
|   GenericLines: 
|     HTTP/1.0 400 Bad Request
|     Date: Thu, 01 Jul 2021 00:42:34 GMT
|     Content-Length: 22
|     Content-Type: text/plain; charset=US-ASCII
|     Connection: Close
|     Invalid request line:
|   GetRequest: 
|     HTTP/1.1 412 Precondition Failed
|     Date: Thu, 01 Jul 2021 00:42:34 GMT
|     Content-Length: 0
|   HTTPOptions: 
|     HTTP/1.0 501 Not Implemented
|     Date: Thu, 01 Jul 2021 00:42:40 GMT
|     Content-Length: 29
|     Content-Type: text/plain; charset=US-ASCII
|     Connection: Close
|     Method not supported: OPTIONS
|   Help: 
|     HTTP/1.0 400 Bad Request
|     Date: Thu, 01 Jul 2021 00:42:55 GMT
|     Content-Length: 26
|     Content-Type: text/plain; charset=US-ASCII
|     Connection: Close
|     Invalid request line: HELP
|   RTSPRequest: 
|     HTTP/1.0 400 Bad Request
|     Date: Thu, 01 Jul 2021 00:42:40 GMT
|     Content-Length: 39
|     Content-Type: text/plain; charset=US-ASCII
|     Connection: Close
|     valid protocol version: RTSP/1.0
|   SSLSessionReq: 
|     HTTP/1.0 400 Bad Request
|     Date: Thu, 01 Jul 2021 00:42:55 GMT
|     Content-Length: 73
|     Content-Type: text/plain; charset=US-ASCII
|     Connection: Close
|     Invalid request line: 
|     ?G???,???`~?
|     ??{????w????<=?o?
|   TLSSessionReq: 
|     HTTP/1.0 400 Bad Request
|     Date: Thu, 01 Jul 2021 00:42:55 GMT
|     Content-Length: 71
|     Content-Type: text/plain; charset=US-ASCII
|     Connection: Close
|     Invalid request line: 
|     ??random1random2random3random4
|   TerminalServerCookie: 
|     HTTP/1.0 400 Bad Request
|     Date: Thu, 01 Jul 2021 00:42:55 GMT
|     Content-Length: 54
|     Content-Type: text/plain; charset=US-ASCII
|     Connection: Close
|     Invalid request line: 
|_    Cookie: mstshash=nmap
59777/tcp open     http    Bukkit JSONAPI httpd for Minecraft game server 3.6.0 or older
|_http-title: Site doesn't have a title (text/plain).
2 services unrecognized despite returning data. 
If you know the service/version, please submit the following fingerprints 
at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port2222-TCP:V=7.91%I=7%D=6/30%Time=60DD0CC2%P=x86_64-pc-linux-gnu%r(NU
SF:LL,24,"SSH-2\.0-SSH\x20Server\x20-\x20Banana\x20Studio\r\n");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port39647-TCP:V=7.91%I=7%D=6/30%Time=60DD0CC1%P=x86_64-pc-linux-gnu%r(G
SF:enericLines,AA,"HTTP/1\.0\x20400\x20Bad\x20Request\r\nDate:\x20Thu,\x20
SF:01\x20Jul\x202021\x2000:42:34\x20GMT\r\nContent-Length:\x2022\r\nConten
SF:t-Type:\x20text/plain;\x20charset=US-ASCII\r\nConnection:\x20Close\r\n\
SF:r\nInvalid\x20request\x20line:\x20")%r(GetRequest,5C,"HTTP/1\.1\x20412\
SF:x20Precondition\x20Failed\r\nDate:\x20Thu,\x2001\x20Jul\x202021\x2000:4
SF:2:34\x20GMT\r\nContent-Length:\x200\r\n\r\n")%r(HTTPOptions,B5,"HTTP/1\
SF:.0\x20501\x20Not\x20Implemented\r\nDate:\x20Thu,\x2001\x20Jul\x202021\x
SF:2000:42:40\x20GMT\r\nContent-Length:\x2029\r\nContent-Type:\x20text/pla
SF:in;\x20charset=US-ASCII\r\nConnection:\x20Close\r\n\r\nMethod\x20not\x2
SF:0supported:\x20OPTIONS")%r(RTSPRequest,BB,"HTTP/1\.0\x20400\x20Bad\x20R
SF:equest\r\nDate:\x20Thu,\x2001\x20Jul\x202021\x2000:42:40\x20GMT\r\nCont
SF:ent-Length:\x2039\r\nContent-Type:\x20text/plain;\x20charset=US-ASCII\r
SF:\nConnection:\x20Close\r\n\r\nNot\x20a\x20valid\x20protocol\x20version:
SF:\x20\x20RTSP/1\.0")%r(Help,AE,"HTTP/1\.0\x20400\x20Bad\x20Request\r\nDa
SF:te:\x20Thu,\x2001\x20Jul\x202021\x2000:42:55\x20GMT\r\nContent-Length:\
SF:x2026\r\nContent-Type:\x20text/plain;\x20charset=US-ASCII\r\nConnection
SF::\x20Close\r\n\r\nInvalid\x20request\x20line:\x20HELP")%r(SSLSessionReq
SF:,DD,"HTTP/1\.0\x20400\x20Bad\x20Request\r\nDate:\x20Thu,\x2001\x20Jul\x
SF:202021\x2000:42:55\x20GMT\r\nContent-Length:\x2073\r\nContent-Type:\x20
SF:text/plain;\x20charset=US-ASCII\r\nConnection:\x20Close\r\n\r\nInvalid\
SF:x20request\x20line:\x20\x16\x03\0\0S\x01\0\0O\x03\0\?G\?\?\?,\?\?\?`~\?
SF:\0\?\?{\?\?\?\?w\?\?\?\?<=\?o\?\x10n\0\0\(\0\x16\0\x13\0")%r(TerminalSe
SF:rverCookie,CA,"HTTP/1\.0\x20400\x20Bad\x20Request\r\nDate:\x20Thu,\x200
SF:1\x20Jul\x202021\x2000:42:55\x20GMT\r\nContent-Length:\x2054\r\nContent
SF:-Type:\x20text/plain;\x20charset=US-ASCII\r\nConnection:\x20Close\r\n\r
SF:\nInvalid\x20request\x20line:\x20\x03\0\0\*%\?\0\0\0\0\0Cookie:\x20msts
SF:hash=nmap")%r(TLSSessionReq,DB,"HTTP/1\.0\x20400\x20Bad\x20Request\r\nD
SF:ate:\x20Thu,\x2001\x20Jul\x202021\x2000:42:55\x20GMT\r\nContent-Length:
SF:\x2071\r\nContent-Type:\x20text/plain;\x20charset=US-ASCII\r\nConnectio
SF:n:\x20Close\r\n\r\nInvalid\x20request\x20line:\x20\x16\x03\0\0i\x01\0\0
SF:e\x03\x03U\x1c\?\?random1random2random3random4\0\0\x0c\0/\0");

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Jun 30 19:32:33 2021 -- 1 IP address (1 host up) scanned in 103.94 seconds
```

## Gobuster against port 59777
```bash
hunter@kali ~/htb/explore$ gobuster dir -u http://10.10.10.247:59777 -w /opt/SecLists/Discovery/Web-Content/raft-small-words.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.10.247:59777
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /opt/SecLists/Discovery/Web-Content/raft-small-words.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/06/30 19:37:40 Starting gobuster in directory enumeration mode
===============================================================
/cache                (Status: 301) [Size: 67] [--> /cache/]
/bin                  (Status: 301) [Size: 63] [--> /bin/]  
/data                 (Status: 301) [Size: 65] [--> /data/] 
/config               (Status: 301) [Size: 69] [--> /config/]
/lib                  (Status: 301) [Size: 63] [--> /lib/]   
/product              (Status: 301) [Size: 71] [--> /product/]
/dev                  (Status: 301) [Size: 63] [--> /dev/]    
/system               (Status: 301) [Size: 69] [--> /system/] 
/.                    (Status: 301) [Size: 59] [--> /./]      
/init                 (Status: 403) [Size: 31]                
/d                    (Status: 301) [Size: 59] [--> /d/]      
/etc                  (Status: 301) [Size: 63] [--> /etc/]    
/sys                  (Status: 301) [Size: 63] [--> /sys/]    
/storage              (Status: 301) [Size: 71] [--> /storage/]
/vendor               (Status: 301) [Size: 69] [--> /vendor/] 
/proc                 (Status: 301) [Size: 65] [--> /proc/]   
/oem                  (Status: 301) [Size: 63] [--> /oem/]    
/sbin                 (Status: 301) [Size: 65] [--> /sbin/]   
/acct                 (Status: 301) [Size: 65] [--> /acct/]   
/mnt                  (Status: 301) [Size: 63] [--> /mnt/]    
                                                              
===============================================================
2021/06/30 19:46:04 Finished
===============================================================
```
* This looks suspiciously like the root directory for a linux-based box...
* When visiting any of these pages, get this:

![[Pasted image 20210630195548.png]]
* Can change message by posting json data

![[Pasted image 20210630195815.png]]
* JSON injection..?