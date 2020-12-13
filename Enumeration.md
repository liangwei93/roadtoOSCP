Nmap scan
nmap -sV -sC -o nmap 10.10.85.80


Gobuster 
gobuster dir -e -u 10.10.243.102 -x php,txt,cgi,css,py,html -w /usr/share/dirb/wordlists/common.txt

wildcard related error
gobuster dir -e -u http://10.10.20.239/api/site-log.php -x php,txt,cgi,css,py,html -w /usr/share/dirb/wordlists/common.txt -s "204,301,302,307,401,403"


Hydra Brute force
kali@kali:~/oscptraining/tryhackme/brooklyn99$ hydra 10.10.216.12 ssh -l jake -P /usr/share/wordlists/rockyou.txt

SMB Enum (port 445, 139)
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.117.251
smbclient ///10.10.117.251/Users





