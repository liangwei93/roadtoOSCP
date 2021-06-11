------------------------------------------------------------
        Threader 3000 - Multi-threaded Port Scanner          
                       Version 1.0.6                    
                   A project by The Mayor               
------------------------------------------------------------
Enter your target IP address or URL here: 10.10.176.212
------------------------------------------------------------
Scanning target 10.10.176.212
Time started: 2021-06-04 23:32:13.610225
------------------------------------------------------------
Port 22 is open (SSH)
Port 53 is open
Port 135 is open
Port 88 is open
Port 139 is open
Port 389 is open
Port 445 is open
Port 464 is open
Port 593 is open
Port 636 is open
Port 3269 is open
Port 3268 is open
Port 3389 is open (RDP)
Port 9389 is open




PS C:\Users\Administrator\Downloads> Get-NetComputer -fulldata | select operatingsystem

operatingsystem
---------------
Windows Server 2019 Standard
Windows 10 Enterprise Evaluation
Windows 10 Enterprise Evaluation

PS C:\Users\Administrator\Downloads> Get-NetUser | select cn

cn                  
--
Administrator
Guest
krbtgt
Machine-1
Admin2
Machine-2
SQL Service
POST{P0W3RV13W_FTW} 
sshd

PS C:\Users\Administrator\Downloads> net localgroup

Aliases for \\DOMAIN-CONTROLL

-------------------------------------------------------------------------------
*Access Control Assistance Operators
*Account Operators
*Administrators
*Allowed RODC Password Replication Group
*Backup Operators
*Cert Publishers
*Certificate Service DCOM Access
*Cryptographic Operators
*Denied RODC Password Replication Group
*Distributed COM Users
*DnsAdmins
*Event Log Readers
*Guests
*Hyper-V Administrators
*IIS_IUSRS
*Incoming Forest Trust Builders
*Network Configuration Operators
*Performance Log Users
*Performance Monitor Users
*Pre-Windows 2000 Compatible Access
*Print Operators
*RAS and IAS Servers
*RDS Endpoint Servers
*RDS Management Servers
*RDS Remote Access Servers
*Remote Desktop Users
*Remote Management Users
*Replicator
*Server Operators
*Storage Replica Administrators
*Terminal Server License Servers
*Users
*Windows Authorization Access Group
The command completed successfully.


PS C:\Users\Administrator\Downloads> Get-ADUser -identity SQLService -properties *

PasswordLastSet                      : 5/13/2020 8:26:58 PM


