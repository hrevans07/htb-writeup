## Nmap
```bash
# Nmap 7.92 scan initiated Wed Apr 20 21:17:32 2022 as: nmap -sC -sV -oA nmap/init 10.10.11.152
Nmap scan report for 10.10.11.152
Host is up (0.040s latency).
Not shown: 991 filtered tcp ports (no-response)
PORT    STATE SERVICE       VERSION
53/tcp  open  domain        Simple DNS Plus
88/tcp  open  kerberos-sec  Microsoft Windows Kerberos (server time: 2022-04-21 10:25:26Z)
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: timelapse.htb0., Site: Default-First-Site-Name)
445/tcp open  microsoft-ds?
464/tcp open  kpasswd5?
593/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp open  ldapssl?
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 8h07m42s
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2022-04-21T10:25:36
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Apr 20 21:18:39 2022 -- 1 IP address (1 host up) scanned in 66.91 seconds
```
## SMB
Shares w/ anonymous login
```powershell
smbclient  -L \\\\10.10.11.152\\   
Password for [WORKGROUP\hrevans]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        Shares          Disk      
        SYSVOL          Disk      Logon server share
```
The 'Shares' share allows for grabbing files
```bash
$ ls smb_files                     
LAPS_Datasheet.docx  LAPS_OperationsGuide.docx  LAPS_TechnicalSpecification.docx  LAPS.x64.msi  winrm_backup.zip
```
The zip is password protected...