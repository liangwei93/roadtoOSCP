nmap -sV -sC -o nmap 10.10.225.194

noted that port 22 ssh and port 80 http is open
server is running on linux

side note; i just switched over from virtualbox to vmware 12 free version for mac, and it's smooth af. really wondering what was i doing on virtualbox previously....

next up explore the page on firefox, first thing i do is to view page source to see if there's anything hidden in there.
nothing much over there though.
exploring the downloads page seems to show nothing much too.
about us page seems to show the names of some staff; possible usernames for ssh login maybe?
best bet now is to use gobuster (read that it is faster than dirb and seems to think so when i used it for some boxes)

as this is a fresh install on vmware, i encountered this same problem i faced on virtual box "E: Unable to locate package" when i tried to install gobuster
following the following to resolve 
https://ourcodeworld.com/articles/read/961/how-to-solve-kali-linux-apt-get-install-e-unable-to-locate-package-checkinstall


gobuster dir -e -u http://10.10.225.194/ -w /usr/share/wordlists/dirb/common.txt

these are the results: 
http://10.10.225.194/aboutus (Status: 301)
http://10.10.225.194/admin (Status: 301)
http://10.10.225.194/css (Status: 301)
http://10.10.225.194/downloads (Status: 301)
http://10.10.225.194/img (Status: 301)
http://10.10.225.194/index.html (Status: 301)

http://10.10.225.194/admin (Status: 301) < this seems good>

got to the login page with that link, lets try admin/admin probably? failed.
oh the hint says this "OWASP Top 10 Vuln! Do NOT bruteforce."
seems like the credentials shd be hidden somewhere in the page
maybe the source code?

opening the source code shows that they are using rot47
github.com/mitchellh/go-homedir <looks suspicious> nothing in there..
kinda stuck here so lets reverse back to the hint
OWASP TOP 10 
1. Injection (not possible since no sql here)
2. Broken Authentication (seems high likely)
3. Sensitive Data Exposure (not much data here now i guess)
4. XML External Entities (doesnt seem like it)
5. Broken Access Control (likely?)
6. Security Misconfiguration (hmm?)
7. Cross-Site Scripting (not over here i think)
8. Insecure Deserialisation (not here i think)
9. Using Components with Known Vulnerabilities (no)
10. Insufficient logging and monitoring (not here)

This really seems like the most likely. 
2. Broken Authentication (seems high likely) 

back to the login page, view page source, theres this file called login.js and theres an if else function in it that seems to check for some cookies.
refered to this page for help 
https://medium.com/@jonaldallan/write-up-try-hack-me-overpass-2ace3e3ba806


081020 0207am got into the box with james credentials, lets leave privilege escalation for tmr then........
