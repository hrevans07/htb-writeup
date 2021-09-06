## Nmap
```bash
sudo portscan <insert ip>
```
 * portscan is a bash script I wrote that automatically does a full port scan then scans open ports to enumerate versions and scripts
	 * It basically does this:
```bash
sudo nmap -p- -oA nmap/all_ports <insert ip>
sudo nmap -p <ports found in first nmap> -sC -sV -oA nmap/final <insert ip>
```
## SNMP
* SNMP is a fairly common network manager application
 ```bash
 snmpwalk -c public -v *version* *ip*
 ```
*Note: there is a way to get more output than with this command, I haven't figured out how yet*
```bash 
snmp-check *ip*
```
## DNS
- nslookup
	- Use it to maybe leak hostname if port 53 is open
	- Try looking for other things:
		- localhost
		- its own ip address (i.e. 10.10.10.248)
```bash
hunter@kali ~/htb/intelligence$ nslookup
> server 10.10.10.248
Default server: 10.10.10.248
Address: 10.10.10.248#53
> 127.0.0.1
1.0.0.127.in-addr.arpa  name = localhost.
> localhost
Server:         10.10.10.248
Address:        10.10.10.248#53
```
- dig
	- Use it for a dns zone transfer attack, which can leak more information about the box

```bash
hunter@kali ~/htb/friendzone$ dig axfr @10.10.10.123 friendzone.red

; <<>> DiG 9.16.15-Debian <<>> axfr @10.10.10.123 friendzone.red
; (1 server found)
;; global options: +cmd
friendzone.red.		604800	IN	SOA	localhost. root.localhost. 2 604800 86400 2419200 604800
friendzone.red.		604800	IN	AAAA	::1
friendzone.red.		604800	IN	NS	localhost.
friendzone.red.		604800	IN	A	127.0.0.1
administrator1.friendzone.red. 604800 IN A	127.0.0.1
hr.friendzone.red.	604800	IN	A	127.0.0.1
uploads.friendzone.red.	604800	IN	A	127.0.0.1
friendzone.red.		604800	IN	SOA	localhost. root.localhost. 2 604800 86400 2419200 604800
;; Query time: 52 msec
;; SERVER: 10.10.10.123#53(10.10.10.123)
;; WHEN: Sat Jul 10 11:17:52 CDT 2021
;; XFR size: 8 records (messages 1, bytes 289)
```
## Privesc
* Linux command to find all files modified on a certain date
	* Note can also be used  to find files meeting different criteriea, just change grep arg

```bash
find / -type f -ls 2>/dev/null | grep -i 'jun 21'
```

* Linux command to search every file in directory for a string
	* -r is recursive
	* -n is line number
	* -w is match whole word
	* -e is pattern to search for
	* Note: **2>/dev/null** after the grep is probably necessary
```bash
grep -rnw '/path/to/somewhere/' -e 'pattern'
```
