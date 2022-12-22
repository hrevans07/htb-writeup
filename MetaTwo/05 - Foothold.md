## SQLi
Doing some manual enumeration of the app shows that the app is WordPress.
The usual foothold with WP is through the plugins that are installed, but looking at /wp-content/plugins shows just a blank page...
Do some mapping of the web app and eventually find a calendar/appointment functionality that is the WP plugin ```BookingPress``` which has a SQLi vulnerability: https://wpscan.com/vulnerability/388cd42d-b61a-42a4-8604-99b812db2357
![[Pasted image 20221107153617.png]]
This injection is pretty limited though; the request cannot have any single or double quotes, which makes it pretty difficult to do anything.
Extract passwords for 2 users: manager and admin
![[Pasted image 20221107153906.png]]
The manager user's hash can be cracked: ```manager:partylikearockstar```
This gives a login to WP as a low privilege user.