## Nmap
```bash
# Nmap 7.91 scan initiated Thu Jul 22 15:24:05 2021 as: nmap -sC -sV -oA nmap/init 10.10.10.238
Nmap scan report for 10.10.10.238
Host is up (0.12s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ba:cc:cd:81:fc:91:55:f3:f6:a9:1f:4e:e8:be:e5:2e (RSA)
|   256 69:43:37:6a:18:09:f5:e7:7a:67:b8:18:11:ea:d7:65 (ECDSA)
|_  256 5d:5e:3f:67:ef:7d:76:23:15:11:4b:53:f8:41:3a:94 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Site doesn't have a title (text/html; charset=iso-8859-1).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Jul 22 15:24:28 2021 -- 1 IP address (1 host up) scanned in 22.70 seconds
```
## Wordpress
Visiting the web server shows that this is a wordpress site. Try running wpscan on it.
```sql
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.18
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[32m[+][0m URL: http://monitors.htb/ [10.10.10.238]
[32m[+][0m Started: Thu Jul 22 15:34:51 2021

Interesting Finding(s):

[32m[+][0m Headers
 | Interesting Entry: Server: Apache/2.4.29 (Ubuntu)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[32m[+][0m XML-RPC seems to be enabled: http://monitors.htb/xmlrpc.php
 | Found By: Link Tag (Passive Detection)
 | Confidence: 100%
 | Confirmed By: Direct Access (Aggressive Detection), 100% confidence
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/

[32m[+][0m WordPress readme found: http://monitors.htb/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[32m[+][0m Upload directory has listing enabled: http://monitors.htb/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[32m[+][0m The external WP-Cron seems to be enabled: http://monitors.htb/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[32m[+][0m WordPress version 5.5.1 identified (Insecure, released on 2020-09-01).
 | Found By: Rss Generator (Passive Detection)
 |  - http://monitors.htb/index.php/feed/, <generator>https://wordpress.org/?v=5.5.1</generator>
 |  - http://monitors.htb/index.php/comments/feed/, <generator>https://wordpress.org/?v=5.5.1</generator>
 |
 | [31m[!][0m 10 vulnerabilities identified:
 |
 | [31m[!][0m Title: WordPress < 5.5.2 - Hardening Deserialization Requests
 |     Fixed in: 5.5.2
 |     References:
 |      - https://wpscan.com/vulnerability/f2bd06cf-f4e9-4077-90b0-fba80c3d0969
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28032
 |      - https://wordpress.org/news/2020/10/wordpress-5-5-2-security-and-maintenance-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/add6bedf3a53b647d0ebda2970057912d3cd79d3
 |      - https://blog.wpscan.com/2020/10/30/wordpress-5.5.2-security-release.html
 |      - https://github.com/WordPress/Requests/security/advisories/GHSA-52qp-jpq7-6c54
 |
 | [31m[!][0m Title: WordPress < 5.5.2 - Disable Spam Embeds from Disabled Sites on a Multisite Network
 |     Fixed in: 5.5.2
 |     References:
 |      - https://wpscan.com/vulnerability/a1941f4f-6adb-41e9-b47f-6eddd6f6a04a
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28033
 |      - https://wordpress.org/news/2020/10/wordpress-5-5-2-security-and-maintenance-release/
 |      - https://blog.wpscan.com/2020/10/30/wordpress-5.5.2-security-release.html
 |
 | [31m[!][0m Title: WordPress < 5.5.2 - Cross-Site Scripting (XSS) via Global Variables
 |     Fixed in: 5.5.2
 |     References:
 |      - https://wpscan.com/vulnerability/336deb2e-5286-422d-9aa2-6898877d55a9
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28034
 |      - https://wordpress.org/news/2020/10/wordpress-5-5-2-security-and-maintenance-release/
 |      - https://blog.wpscan.com/2020/10/30/wordpress-5.5.2-security-release.html
 |
 | [31m[!][0m Title: WordPress < 5.5.2 - XML-RPC Privilege Escalation
 |     Fixed in: 5.5.2
 |     References:
 |      - https://wpscan.com/vulnerability/76a05ec0-08f3-459f-8379-3b4865a0813f
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28035
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28036
 |      - https://wordpress.org/news/2020/10/wordpress-5-5-2-security-and-maintenance-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/c9e6b98968025b1629015998d12c3102165a7d32
 |      - https://blog.wpscan.com/2020/10/30/wordpress-5.5.2-security-release.html
 |
 | [31m[!][0m Title: WordPress < 5.5.2 - Unauthenticated DoS Attack to RCE
 |     Fixed in: 5.5.2
 |     References:
 |      - https://wpscan.com/vulnerability/016774df-5031-4315-a893-a47d99273883
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28037
 |      - https://wordpress.org/news/2020/10/wordpress-5-5-2-security-and-maintenance-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/2ca15d1e5ce70493c5c0c096ca0c76503d6da07c
 |      - https://blog.wpscan.com/2020/10/30/wordpress-5.5.2-security-release.html
 |      - https://threatpost.com/wordpress-patches-rce-bug/160812/
 |
 | [31m[!][0m Title: WordPress < 5.5.2 - Stored XSS in Post Slugs
 |     Fixed in: 5.5.2
 |     References:
 |      - https://wpscan.com/vulnerability/990cf4ff-0084-4a5c-8fdb-db374ffcb5df
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28038
 |      - https://wordpress.org/news/2020/10/wordpress-5-5-2-security-and-maintenance-release/
 |      - https://blog.wpscan.com/2020/10/30/wordpress-5.5.2-security-release.html
 |
 | [31m[!][0m Title: WordPress < 5.5.2 - Protected Meta That Could Lead to Arbitrary File Deletion
 |     Fixed in: 5.5.2
 |     References:
 |      - https://wpscan.com/vulnerability/30662254-5a8d-40d0-8a31-eb58b51b3c33
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28039
 |      - https://wordpress.org/news/2020/10/wordpress-5-5-2-security-and-maintenance-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/d5ddd6d4be1bc9fd16b7796842e6fb26315705ad
 |      - https://blog.wpscan.com/2020/10/30/wordpress-5.5.2-security-release.html
 |
 | [31m[!][0m Title: WordPress < 5.5.2 - Cross-Site Request Forgery (CSRF) to Change Theme Background
 |     Fixed in: 5.5.2
 |     References:
 |      - https://wpscan.com/vulnerability/ebd354db-ab63-4644-891c-4a200e9eef7e
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28040
 |      - https://wordpress.org/news/2020/10/wordpress-5-5-2-security-and-maintenance-release/
 |      - https://github.com/WordPress/wordpress-develop/commit/cbcc595974d5aaa025ca55625bf68ef286bd8b41
 |      - https://blog.wpscan.com/2020/10/30/wordpress-5.5.2-security-release.html
 |      - https://hackerone.com/reports/881855
 |
 | [31m[!][0m Title: WordPress 4.7-5.7 - Authenticated Password Protected Pages Exposure
 |     Fixed in: 5.5.4
 |     References:
 |      - https://wpscan.com/vulnerability/6a3ec618-c79e-4b9c-9020-86b157458ac5
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-29450
 |      - https://wordpress.org/news/2021/04/wordpress-5-7-1-security-and-maintenance-release/
 |      - https://blog.wpscan.com/2021/04/15/wordpress-571-security-vulnerability-release.html
 |      - https://github.com/WordPress/wordpress-develop/security/advisories/GHSA-pmmh-2f36-wvhq
 |      - https://core.trac.wordpress.org/changeset/50717/
 |      - https://www.youtube.com/watch?v=J2GXmxAdNWs
 |
 | [31m[!][0m Title: WordPress 3.7 to 5.7.1 - Object Injection in PHPMailer
 |     Fixed in: 5.5.5
 |     References:
 |      - https://wpscan.com/vulnerability/4cd46653-4470-40ff-8aac-318bee2f998d
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-36326
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19296
 |      - https://github.com/WordPress/WordPress/commit/267061c9595fedd321582d14c21ec9e7da2dcf62
 |      - https://wordpress.org/news/2021/05/wordpress-5-7-2-security-release/
 |      - https://github.com/PHPMailer/PHPMailer/commit/e2e07a355ee8ff36aba21d0242c5950c56e4c6f9
 |      - https://www.wordfence.com/blog/2021/05/wordpress-5-7-2-security-release-what-you-need-to-know/
 |      - https://www.youtube.com/watch?v=HaW15aMzBUM

[32m[+][0m WordPress theme in use: iconic-one
 | Location: http://monitors.htb/wp-content/themes/iconic-one/
 | Last Updated: 2021-06-15T00:00:00.000Z
 | Readme: http://monitors.htb/wp-content/themes/iconic-one/readme.txt
 | [33m[!][0m The version is out of date, the latest version is 2.2
 | Style URL: http://monitors.htb/wp-content/themes/iconic-one/style.css?ver=1.7.8
 | Style Name: Iconic One
 | Style URI: https://themonic.com/iconic-one/
 | Description: Iconic One is a premium quality theme with pixel perfect typography and responsiveness and is built ...
 | Author: Themonic
 | Author URI: https://themonic.com
 |
 | Found By: Css Style In Homepage (Passive Detection)
 |
 | Version: 2.1.7 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://monitors.htb/wp-content/themes/iconic-one/style.css?ver=1.7.8, Match: 'Version: 2.1.7'


[34m[i][0m Plugin(s) Identified:

[32m[+][0m wp-with-spritz
 | Location: http://monitors.htb/wp-content/plugins/wp-with-spritz/
 | Latest Version: 1.0 (up to date)
 | Last Updated: 2015-08-20T20:15:00.000Z
 |
 | Found By: Urls In Homepage (Passive Detection)
 |
 | [31m[!][0m 1 vulnerability identified:
 |
 | [31m[!][0m Title: WP with Spritz 1.0 - Unauthenticated File Inclusion
 |     References:
 |      - https://wpscan.com/vulnerability/cdd8b32a-b424-4548-a801-bbacbaad23f8
 |      - https://www.exploit-db.com/exploits/44544/
 |
 | Version: 4.2.4 (80% confidence)
 | Found By: Readme - Stable Tag (Aggressive Detection)
 |  - http://monitors.htb/wp-content/plugins/wp-with-spritz/readme.txt


[34m[i][0m No Config Backups Found.

[32m[+][0m WPScan DB API OK
 | Plan: free
 | Requests Done (during the scan): 0
 | Requests Remaining: 22

[32m[+][0m Finished: Thu Jul 22 15:34:58 2021
[32m[+][0m Requests Done: 141
[32m[+][0m Cached Requests: 41
[32m[+][0m Data Sent: 34.975 KB
[32m[+][0m Data Received: 21.599 KB
[32m[+][0m Memory used: 210.746 MB
[32m[+][0m Elapsed time: 00:00:07

```