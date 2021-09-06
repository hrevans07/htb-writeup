Doing some manual enumeration, find a backup directory in /var that we can read
```bash
www-data@thenotebook:/var/backups$ ls -la
total 60
drwxr-xr-x  2 root root  4096 Jul  8 01:48 .
drwxr-xr-x 14 root root  4096 Feb 12 06:52 ..
-rw-r--r--  1 root root 33252 Feb 24 08:53 apt.extended_states.0
-rw-r--r--  1 root root  3609 Feb 23 08:58 apt.extended_states.1.gz
-rw-r--r--  1 root root  3621 Feb 12 06:52 apt.extended_states.2.gz
-rw-r--r--  1 root root  4373 Feb 17 09:02 home.tar.gz
www-data@thenotebook:/var/backups$
```
home.tar.gz looks interesting

Unzip and untar it to see its a backup of the /home directory....complete with a .ssh directory for noah

There's no way the id_rsa still works with ssh right?
```bash
hunter@kali ~/htb/thenotebook/www/home/noah/.ssh$ ssh -i id_rsa noah@10.10.10.230
[snip]
noah@thenotebook:~$
```