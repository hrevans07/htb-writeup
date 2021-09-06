## Nmap 
```bash
# Nmap 7.91 scan initiated Thu Jun 10 13:08:00 2021 as: nmap -p 80, -sC -sV -oA nmap/final 10.10.10.68
Nmap scan report for 10.10.10.68
Host is up (0.048s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Arrexel's Development Site

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Jun 10 13:08:09 2021 -- 1 IP address (1 host up) scanned in 8.61 seconds
```
* Do a gobuster and it reveals some interresting directories:
![[Pasted image 20210610131432.png]]
* Look inside of the dev directory, and find a file named "phpbash.php"
* This is pretty much just a webshell, which gives us code execution on the server as www-data
![[Pasted image 20210610131601.png]]