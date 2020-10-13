kali@kali:~/oscptraining/tryhackme/Ignite$ nmap -sV -sC -o nmap 10.10.235.151
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-13 12:12 EDT
Nmap scan report for 10.10.235.151
Host is up (0.35s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/fuel/
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Welcome to FUEL CMS

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 40.63 seconds
kali@kali:~/oscptraining/tryhackme/Ignite$ gobuster dir -e -u http://10.10.235.151/ -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.235.151/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Expanded:       true
[+] Timeout:        10s
===============================================================
2020/10/13 12:14:11 Starting gobuster
===============================================================
http://10.10.235.151/.hta (Status: 403)
http://10.10.235.151/.htaccess (Status: 403)
http://10.10.235.151/.htpasswd (Status: 403)
http://10.10.235.151/0 (Status: 200)
http://10.10.235.151/assets (Status: 301)
http://10.10.235.151/home (Status: 200)
http://10.10.235.151/index.php (Status: 200)
http://10.10.235.151/index (Status: 200)
http://10.10.235.151/offline (Status: 200)
http://10.10.235.151/robots.txt (Status: 200)
http://10.10.235.151/server-status (Status: 403)
===============================================================
2020/10/13 12:18:08 Finished                                                                                                            
===============================================================  

kali@kali:~/oscptraining/tryhackme/Ignite$ searchsploit 47138 -p
  Exploit: fuelCMS 1.4.1 - Remote Code Execution
      URL: https://www.exploit-db.com/exploits/47138
     Path: /usr/share/exploitdb/exploits/linux/webapps/47138.py
File Type: HTML document, ASCII text, with CRLF line terminators
