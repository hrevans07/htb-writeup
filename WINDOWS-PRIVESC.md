# Service exploits
- use executable like *https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk* to check user account permissions
	- ```SERVICE_QUERY_*``` means you can use the sc.exe to get more info on the process, in this case daclsvc
	- ```SERVICE_START, SERVICE_STOP``` means you can use net to restart service, which is necessary if you're trying to inject a binary
```powershell
.\accesschk.exe /accepteula -uwcqv user *
RW daclsvc
        SERVICE_QUERY_STATUS
        SERVICE_QUERY_CONFIG
        SERVICE_CHANGE_CONFIG
        SERVICE_INTERROGATE
        SERVICE_ENUMERATE_DEPENDENTS
        SERVICE_START
        SERVICE_STOP
        READ_CONTROL
```
- use ```sc.exe qc <service>``` to query service and see who it is ran as and what it is running
	- look for ```SYSTEM_START_NAME```, this is the user that it is ran as
	- look for ```BIN_PATH_NAME```, this is what is being ran 
	- note: ```sc.exe <service>``` spits out slightly different info