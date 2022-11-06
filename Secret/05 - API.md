Doing a directory brute force on the web root shows a /api directory
```===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.11.120
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /opt/SecLists/Discovery/Web-Content/raft-small-words-lowercase.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/12/07 12:25:39 Starting gobuster in directory enumeration mode
===============================================================
/download             (Status: 301) [Size: 183] [--> /download/]
/docs                 (Status: 200) [Size: 20720]               
/api                  (Status: 200) [Size: 93]                  
/assets               (Status: 301) [Size: 179] [--> /assets/]  
/.                    (Status: 301) [Size: 169] [--> /./]       
                                                                
===============================================================
2021/12/07 12:28:39 Finished
===============================================================
```
After some playing around, sending a POST to the /api/user/register page elicits a response other than 404
![[Pasted image 20211207123957.png]]
An error message leaks a user on the box: dasith
![[Pasted image 20211207124346.png]]