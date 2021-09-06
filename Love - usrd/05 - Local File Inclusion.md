* Try visiting staging.love.htb and get this page
![[Pasted image 20210606134636.png]]
* Go to the beta.php page and see there is functionality for uploading files
![[Pasted image 20210606134755.png]]
* Remember from Nmap that there were web servers on the box that just gave 403 forbidden
* Try uploading file localhost:5000
![[Pasted image 20210606134934.png]]
* Find some leaked creds
	* **admin:@LoveIsInTheAir!!!!**

* Creds don't work for signing in on port 80 /index.php page
* Look for an admin login, try /admin/
* Get very similar looking page
![[Pasted image 20210606141628.png]]
* Try logging in on this page with the creds we found -> get valid login
![[Pasted image 20210606141727.png]]
