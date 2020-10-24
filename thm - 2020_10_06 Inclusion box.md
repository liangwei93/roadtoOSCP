nmap -sV -sc -o nmap <IP>

noted that there were 2 open ports:
22 - ssh
80 - http

need to get some form of user id before i can ssh in. lets try something else since this is on local file inclusion, we can possibly exploit this vulnerabilty. 

https://www.acunetix.com/blog/articles/local-file-inclusion-lfi/#:~:text=An%20attacker%20can%20use%20Local,to%20a%20file%20as%20input.

noted that a Directory Traversal / Path Traversal attack can be executed with the following address
http://<IP>/?name=../../../../etc/passwd

user falconfeast seems like the most possble account for ssh-ing into the server since it ended with /bin/bash 

the password is :rootpassword as stated on the site...  but im tempted to try out using hydra just for fun. lazy me have already pulled out rock.txt into /home/kali for easy access.

hydra $ip ssh -l <username> -P rockyou.txt    

i let it run in the background while i ssh into the box with the given password.

first thing i do when im in is to run 
ls 
and find the user.txt file

next up running 
sudo -l 
to check the command that i can run as root
/usr/bin/socat

lets proceed to gtfobin to determine how to do privilege escalation with that, the following seems like the best bet

socat stdin exec:/bin/sh

find the root file and thats all!!!
06 Oct 2020; 1155pm

