nmap -sV -sC -o nmap 10.10.10.68                                                                  3

Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-14 11:19 EDT
Nmap scan report for 10.10.10.68
Host is up (0.043s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Arrexel's Development Site

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.69 seconds
