python3 dirsearch.py -u http://10.10.10.15:80/ -e php -x 403,404 -t 50                                       1 ⨯

  _|. _ _  _  _  _ _|_    v0.4.1                                                                                     
 (_||| _) (/_(_|| (_| )                                                                                              
                                                                                                                     
Extensions: php | HTTP method: GET | Threads: 50 | Wordlist size: 8920

Output File: /home/kali/sectools/dirsearch/reports/10.10.10.15/_21-07-03_02-34-37.txt

Error Log: /home/kali/sectools/dirsearch/logs/errors-21-07-03_02-34-37.log

Target: http://10.10.10.15:80/
                                                                                                                     
[02:34:37] Starting: 
[02:34:47] 301 -  155B  - /_vti_bin  ->  http://10.10.10.15/%5Fvti%5Fbin/                              
[02:34:47] 200 -  759B  - /_vti_bin/            
[02:34:47] 200 -  195B  - /_vti_bin/_vti_adm/admin.dll
[02:34:47] 200 -  195B  - /_vti_bin/_vti_aut/author.dll
[02:34:47] 200 -   96B  - /_vti_bin/shtml.dll
[02:34:47] 200 -  105B  - /_vti_bin/shtml.dll/asdfghjkl
[02:34:47] 200 -  106B  - /_vti_bin/shtml.exe/qwertyuiop
[02:34:47] 200 -   96B  - /_vti_bin/shtml.exe?_vti_rpc
                                                           


PORT   STATE SERVICE VERSION
80/tcp open  http    Microsoft IIS httpd 6.0
| http-methods: 
|_  Potentially risky methods: TRACE DELETE COPY MOVE PROPFIND PROPPATCH SEARCH MKCOL LOCK UNLOCK PUT
|_http-server-header: Microsoft-IIS/6.0
|_http-title: Error
| http-webdav-scan: 
|   WebDAV type: Unknown
|   Public Options: OPTIONS, TRACE, GET, HEAD, DELETE, PUT, POST, COPY, MOVE, MKCOL, PROPFIND, PROPPATCH, LOCK, UNLOCK, SEARCH
|   Server Date: Sat, 03 Jul 2021 06:29:48 GMT
|   Allowed Methods: OPTIONS, TRACE, GET, HEAD, DELETE, COPY, MOVE, PROPFIND, PROPPATCH, SEARCH, MKCOL, LOCK, UNLOCK
|_  Server Type: Microsoft-IIS/6.0
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows



whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeAuditPrivilege              Generate security audits                  Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 

c:\windows\system32\inetsrv>systeminfo
systeminfo

Host Name:                 GRANNY
OS Name:                   Microsoft(R) Windows(R) Server 2003, Standard Edition
OS Version:                5.2.3790 Service Pack 2 Build 3790
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Server
OS Build Type:             Uniprocessor Free
Registered Owner:          HTB
Registered Organization:   HTB
Product ID:                69712-296-0024942-44782
Original Install Date:     4/12/2017, 5:07:40 PM
System Up Time:            0 Days, 0 Hours, 45 Minutes, 47 Seconds
System Manufacturer:       VMware, Inc.
System Model:              VMware Virtual Platform
System Type:               X86-based PC
Processor(s):              1 Processor(s) Installed.
                           [01]: x86 Family 6 Model 79 Stepping 1 GenuineIntel ~2099 Mhz
BIOS Version:              INTEL  - 6040000
Windows Directory:         C:\WINDOWS
System Directory:          C:\WINDOWS\system32
Boot Device:               \Device\HarddiskVolume1
System Locale:             en-us;English (United States)
Input Locale:              en-us;English (United States)
Time Zone:                 (GMT+02:00) Athens, Beirut, Istanbul, Minsk
Total Physical Memory:     1,023 MB
Available Physical Memory: 787 MB
Page File: Max Size:       2,470 MB
Page File: Available:      2,325 MB
Page File: In Use:         145 MB
Page File Location(s):     C:\pagefile.sys
Domain:                    HTB
Logon Server:              N/A
Hotfix(s):                 1 Hotfix(s) Installed.
                           [01]: Q147222
Network Card(s):           N/A


i got problem transferring from attacker to victim
