Try using smbclient with these new creds to enumerate the shares

```bash
hunter@kali [04:27:37 PM] [~/htb/intelligence] 
-> % smbclient -U 'Tiffany.Molina' -L \\10.10.10.248
Enter WORKGROUP\Tiffany.Molina's password: 

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        IT              Disk      
        NETLOGON        Disk      Logon server share 
        SYSVOL          Disk      Logon server share 
        Users           Disk      
SMB1 disabled -- no workgroup available
```
We have read in "IT" and "Users" shares
* IT contains a powershell script
* Users appears to be the C:\users folder of a windows machine
* Mount the share, go in to /Tiffany.Molina/Desktop and get the user flag

## IT share ps1 script
*downdetector.ps1*
```powershell
# Check web server status. Scheduled to run every 5min
Import-Module ActiveDirectory 
foreach($record in Get-ChildItem "AD:DC=intelligence.htb,CN=MicrosoftDNS,DC=DomainDnsZones,DC=intelligence,DC=htb" | Where-Object Name -like "web*")  {
try {
$request = Invoke-WebRequest -Uri "http://$($record.Name)" -UseDefaultCredentials
if(.StatusCode -ne 200) {
Send-MailMessage -From 'Ted Graves <Ted.Graves@intelligence.htb>' -To 'Ted Graves <Ted.Graves@intelligence.htb>' -Subject "Host: $($record.Name) is down"
}
} catch {}
}
```