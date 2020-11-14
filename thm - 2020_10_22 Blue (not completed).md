kali@kali:~/oscptraining/tryhackme/Blue$ nmap -sV -sC -o nmap 10.10.243.89
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-22 07:42 EDT
Nmap scan report for 10.10.243.89
Host is up (0.32s latency).
Not shown: 991 closed ports
PORT      STATE SERVICE            VERSION
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  ssl/ms-wbt-server?
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49158/tcp open  msrpc              Microsoft Windows RPC
49159/tcp open  msrpc              Microsoft Windows RPC
Service Info: Host: JON-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h40m00s, deviation: 2h53m12s, median: 0s
|_nbstat: NetBIOS name: JON-PC, NetBIOS user: <unknown>, NetBIOS MAC: 02:92:82:60:e0:57 (unknown)
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: Jon-PC
|   NetBIOS computer name: JON-PC\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2020-10-22T06:43:59-05:00
| smb-security-mode: 
|   account_used: guest                                                                                                                                                                                                                    
|   authentication_level: user                                                                                                                                                                                                             
|   challenge_response: supported                                                                                                                                                                                                          
|_  message_signing: disabled (dangerous, but default)                                                                                                                                                                                     
| smb2-security-mode:                                                                                                                                                                                                                      
|   2.02:                                                                                                                                                                                                                                  
|_    Message signing enabled but not required                                                                                                                                                                                             
| smb2-time:                                                                                                                                                                                                                               
|   date: 2020-10-22T11:43:59                                                                                                                                                                                                              
|_  start_date: 2020-10-22T11:42:06                                                                                                                                                                                                        
                                                                                                                                                                                                                                           
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .                                                                                                                                             
Nmap done: 1 IP address (1 host up) scanned in 159.17 seconds 



kali@kali:~/oscptraining/tryhackme/Blue$ nmap -sV -sC --script vuln 10.10.243.89
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-22 07:49 EDT
Pre-scan script results:
| broadcast-avahi-dos: 
|   Discovered hosts:
|     224.0.0.251
|   After NULL UDP avahi packet DoS (CVE-2011-1002).
|_  Hosts are all up (not vulnerable).
Nmap scan report for 10.10.243.89
Host is up (0.32s latency).
Not shown: 990 closed ports
PORT      STATE    SERVICE            VERSION
37/tcp    filtered time
135/tcp   open     msrpc              Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
139/tcp   open     netbios-ssn        Microsoft Windows netbios-ssn
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
445/tcp   open     microsoft-ds       Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
3389/tcp  open     ssl/ms-wbt-server?
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
| rdp-vuln-ms12-020: 
|   VULNERABLE:
|   MS12-020 Remote Desktop Protocol Denial Of Service Vulnerability
|     State: VULNERABLE
|     IDs:  CVE:CVE-2012-0152
|     Risk factor: Medium  CVSSv2: 4.3 (MEDIUM) (AV:N/AC:M/Au:N/C:N/I:N/A:P)                                                                                                                                                               
|           Remote Desktop Protocol vulnerability that could allow remote attackers to cause a denial of service.                                                                                                                          
|                                                                                                                                                                                                                                          
|     Disclosure date: 2012-03-13                                                                                                                                                                                                          
|     References:                                                                                                                                                                                                                          
|       http://technet.microsoft.com/en-us/security/bulletin/ms12-020                                                                                                                                                                      
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-0152                                                                                                                                                                       
|                                                                                                                                                                                                                                          
|   MS12-020 Remote Desktop Protocol Remote Code Execution Vulnerability                                                                                                                                                                   
|     State: VULNERABLE                                                                                                                                                                                                                    
|     IDs:  CVE:CVE-2012-0002                                                                                                                                                                                                              
|     Risk factor: High  CVSSv2: 9.3 (HIGH) (AV:N/AC:M/Au:N/C:C/I:C/A:C)                                                                                                                                                                   
|           Remote Desktop Protocol vulnerability that could allow remote attackers to execute arbitrary code on the targeted system.                                                                                                      
|                                                                                                                                                                                                                                          
|     Disclosure date: 2012-03-13                                                                                                                                                                                                          
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-0002
|_      http://technet.microsoft.com/en-us/security/bulletin/ms12-020
|_ssl-ccs-injection: No reply from server (TIMEOUT)
|_sslv2-drown: 
49152/tcp open     msrpc              Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
49153/tcp open     msrpc              Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
49154/tcp open     msrpc              Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
49158/tcp open     msrpc              Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
49159/tcp open     msrpc              Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
Service Info: Host: JON-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_samba-vuln-cve-2012-1182: NT_STATUS_ACCESS_DENIED
|_smb-vuln-ms10-054: false
|_smb-vuln-ms10-061: NT_STATUS_ACCESS_DENIED
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|           
|     Disclosure date: 2017-03-14
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
|       https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
|_      https://technet.microsoft.com/en-us/library/security/ms17-010.asp

https://github.com/3ndG4me/AutoBlue-MS17-010

kali@kali:~/oscptraining/tryhackme/Blue/AutoBlue-MS17-010$ python3 eternal_checker.py 10.10.243.89
[*] Target OS: Windows 7 Professional 7601 Service Pack 1
[!] The target is not patched
=== Testing named pipes ===
[*] Done

can use this exploit against our target

did the prerequisite, went in to get the flag, but ended up with some error during privilege escalation. lost connection and still cant connect back, guess i will revisit this again when im in the mood 

221020 2230hr

