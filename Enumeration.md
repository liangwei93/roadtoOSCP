Nmap scan
nmap -sV -sC -o nmap 10.10.85.80


Gobuster 
gobuster dir -e -u 10.10.243.102 -x php,txt,cgi,css,py,html -w /usr/share/dirb/wordlists/common.txt
