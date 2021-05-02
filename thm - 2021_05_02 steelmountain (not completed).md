thm - 2021_05_02 steelmountain.md

Nmap scan
nmap -sV -sC -o nmap 10.10.68.124

gobuster dir -e -u 10.10.68.124 -x php,txt,cgi,css,py,html -w /usr/share/dirb/wordlists/common.txt
