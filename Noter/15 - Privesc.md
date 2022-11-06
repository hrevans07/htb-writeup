### Side note
Check out the db with the creds found in the foothold
```mysql
MariaDB [app]> select username,password from users;
+----------+-------------------------------------------------------------------------------+
| username | password                                                                      |
+----------+-------------------------------------------------------------------------------+
| blue     | $5$rounds=535000$76NyOgtW18b3wIqL$HZqlzNHs1SdzbAb2V6EyAnqYNskA3K.8e1iDesL5vI2 |
| admin    | $5$rounds=535000$mCvaxAvUKjh1ILHQ$OWCsDpqDo3V0lvqWY8/G9koZK4xSAew9oz2t.UskGu2 |
+----------+-------------------------------------------------------------------------------+
```
- This is useless because already have blue's password and admin is the account that I made
## ----------
Remember back to the foothold with downloading the backup zip file -> there was an older backup file that I neglected.
- Grab the older zip and unzip to find the same files as the previous zip, with some different content -> different DB creds
```python
app.config['MYSQL_HOST'] = 'localhost'
app.config['MYSQL_USER'] = 'root'
app.config['MYSQL_PASSWORD'] = 'Nildogg36'
app.config['MYSQL_DB'] = 'app'
app.config['MYSQL_CURSORCLASS'] = 'DictCursor'
```
Use these creds to access the 'app' database
```mysql
MariaDB [mysql]> select Host,User,Password from user;
+-----------+---------+-------------------------------------------+
| Host      | User    | Password                                  |
+-----------+---------+-------------------------------------------+
| localhost | root    | *937440AD99CBB4A102402708AA43B689818489C8 |
| note      | root    |                                           |
| 127.0.0.1 | root    |                                           |
| ::1       | root    |                                           | 
| localhost |         |                                           |
| note      |         |                                           |
| localhost | DB_user | *52107B0F316DEF5A57F59273C66227AEA58A2671 |
+-----------+---------+-------------------------------------------+
```
A hash for the root user? I should probably try and crack this...
