* First things first on box, run sudo -l to see if we can run anything as super user
```bash
User brucetherealadmin may run the following commands on armageddon:
    (root) NOPASSWD: /usr/bin/snap install *
[brucetherealadmin@armageddon ~]$
```
* We can run a binary on the box as super -> check GTFObins

![[Pasted image 20210622130350.png]]
* Well looks pretty good to me
* Sub in whatever command you want to run as root for 'id' and follow the steps
* I normally try and get a root shell on these boxes, but the bash wouldn't work for me for some reason, probably because of special chars or something, and there is no nc on the box -> settle on leaking root flag
	* sub in 'cat /root/root.txt' for 'id' in COMMAND=id