# SQL injection
Navigating to the web server on port 443 reveals a shopping web app:
![[Pasted image 20221022145036.png]]
There is a ton of content on this web app, but it's almost entirely rabbit holes. The most interesting thing is one of the first things seen after landing on the web app: the home page's message saying there is a new checkout process. Put an arbitrary item into the cart and proceed to checkout. It will fail because it is trying to redirect to a different site: ```checkout.shared.htb```.
![[Pasted image 20221022145432.png]]
Add the new subdomain to ```/etc/hosts``` and attempt to checkout again. It reveals a much less polished looking page with some text fields for credit card info and a pay button that does nothing. After checking out the request to the subdomain see that there are two cookies being transmitted, the interesting one being ```custom_cart```. Decoding this cookie shows it's a key value pair representing the cart from the previous site.
![[Pasted image 20221022145834.png]]
Playing with the key value ("CRAAFTKP" in this case) shows that it is vulnerable to blind SQL injection. Unfortunately, this injection point seems to be impossible for SQLmap to exploit, so it must be done manually. I ended up writing a custom python script to brute force the table names character by character which reveals the db has a table for "users". Dumping that table reveals creds for a user "james_mason".
```sql
+----+-------------+----------------------------------+
| id | username    | password                         |
+----+-------------+----------------------------------+
|  1 | james_mason | fc895d4eddc2fc12f995e18c865cf273 |
+----+-------------+----------------------------------+
```
Crack that md5 to get a password that gives an SSH session as user james_mason
```james_mason:Soleil101```
