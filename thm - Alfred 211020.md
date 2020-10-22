kali@kali:~/oscptraining/tryhackme/Alfred$ nmap -sV -sC -o nmap 10.10.22.129
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-21 10:27 EDT
Nmap scan report for 10.10.22.129
Host is up (0.36s latency).
Not shown: 997 filtered ports
PORT     STATE SERVICE            VERSION
80/tcp   open  http               Microsoft IIS httpd 7.5
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/7.5
|_http-title: Site doesn't have a title (text/html).
3389/tcp open  ssl/ms-wbt-server?
|_ssl-date: 2020-10-21T14:28:45+00:00; -1s from scanner time.
8080/tcp open  http               Jetty 9.4.z-SNAPSHOT
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Jetty(9.4.z-SNAPSHOT)
|_http-title: Site doesn't have a title (text/html;charset=utf-8).
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: -1s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 111.14 seconds

nothing much after running gobuster either using common.txt and big.txt

lets look for default pw and user for jenkin
http://10.10.22.129:8080/login?from=%2F
admin;admin 
alright great, got our asses in!

can see that it is running on ver 2.190.1
Page generated: Oct 21, 2020 3:45:35 PM BSTREST APIJenkins ver. 2.190.1
lets check out searchsploit. not too sure about those exploits...
looking at the website we proceed to the project page and build a project


Started by user admin
Running as SYSTEM
Building in workspace C:\Program Files (x86)\Jenkins\workspace\project
[project] $ cmd /c call C:\Users\bruce\AppData\Local\Temp\jenkins8034204804437582227.bat

C:\Program Files (x86)\Jenkins\workspace\project>whoami
alfred\bruce

C:\Program Files (x86)\Jenkins\workspace\project>exit 0 
Finished: SUCCESS

seems like we might be able to do a reverse shell by building a project


