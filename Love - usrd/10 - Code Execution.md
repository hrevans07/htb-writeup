* Can see from our gobuster that there is a /images/ directory
* Notice that image in the /images/ directory is the same as one in the sql database that we dumped earlier
* Try creating a new voter and inserting php code into the photo option?
![[Pasted image 20210606144922.png]]
* Go to /images/test.php?cmd=dir and get code execution
![[Pasted image 20210606145045.png]]
* Craft a new php file that makes file upload easier