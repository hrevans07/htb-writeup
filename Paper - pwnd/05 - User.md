- Look in the 'general' chat room once authenticated and there is reference to a bot
- Start a DM with the bot
- Use the cmd command with the bot to execute arbitrary os commands
	- (Note I found this by using the bots get file functionality to enumerate the bot's source files)
![[Pasted image 20220423151404.png]]
Use this to get a reverse shell -> then create an authorized keys file in .ssh for an easy ssh session.
```bash
ssh -i dwight dwight@10.10.11.143
Activate the web console with: systemctl enable --now cockpit.socket

Last login: Tue Feb  1 09:14:33 2022 from 10.10.14.23
[dwight@paper ~]$ ls
bot_restart.sh  hubot  sales  user.txt
[dwight@paper ~]$
```
