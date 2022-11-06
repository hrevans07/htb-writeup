## Payroll subdomain (rabbit hole)
*Note that everything related to the payroll subdomain is a rabbit hole -> foothold begins with marketing subdomain*
A boolean SQL injection in the password field @ http://preprod-payroll.trick.htb/login.php gives access to the Administrator account of the web app.
![[Pasted image 20220824201509.png]]
Navigate to the "Users" page and use the developer console to unmask the Administrator's password.
![[Pasted image 20220824201630.png]]
```Administrator:Enemigosss:SuperGucciRainbowCake```
## Marketing subdomain
After mapping the marketing subdomain's web application, I noticed that it uses a GET parameter to control content that is returned. 
- So this is most probably a LFI of sorts
![[Pasted image 20220906171209.png]]
# User
Use the LFI that was just found and the information obtained from /etc/passwd to leak a private key for the user Michael
![[Pasted image 20220906171336.png]]