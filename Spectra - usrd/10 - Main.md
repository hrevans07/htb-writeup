## Login
* We know that there is a user named Administrator on this site
* Just got some creds from the /testing site
* Try and login with user Administrator and password we found? 
![[Pasted image 20210618103842.png]]
* Log in to site as user Administrator
* Server is kinda buggy, that's probably on purpose
* Can't edit any themes, no vulnerable plugins
* Let's try and add a theme with vulnerable code
* Create a zip file with the mandatory files for a theme, put malicious php code in the index.php file
* Upload and activate the theme
* Visit index.php and see we have code execution
![[Pasted image 20210618111435.png]]
* Look in /etc/passwd, see that our user (nginx) is allowed a shell
![[Pasted image 20210618123512.png]]
* what's more, we have a .ssh directory in our home
* drop an authorized_keys file in the home directory thats tied to an ssh key we generated, login with ssh as nginx