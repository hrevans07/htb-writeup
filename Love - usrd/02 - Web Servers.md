* There are five different web servers running on this box:
	* port 80
		* has a login.php page that is sql injectable
		* sql injection gives us 2 password hashes that are uncrackable 
	* port 443
		* gives us a 403 forbidden when visiting
	* port 5000
		* gives us a 403 forbidden when visiting
	* port 5985
		* gives us a 404 not found when  visiting
	* port 5986
		* gives us a 404 not found when visiting