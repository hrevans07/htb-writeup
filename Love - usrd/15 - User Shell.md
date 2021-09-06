## Nishang
* Now that we have code execution, try and upload and run a nishang script to give us a good shell
![[Pasted image 20210606151237.png]]
* Edit the Nishang ps1 script that we are uploading to automatiically run by putting the arguments at the bottom of the script
![[Pasted image 20210606151403.png]]
* Then start up a netcat listener on port 9001, hit enter on the burpsuite request, and we have a shell as user phoebe