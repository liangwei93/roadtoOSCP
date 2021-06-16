Hi there, your credentials for our "secret" forum are below :)

username: orestis
password: kIEnnfEKJ#9UmdO

Regards


Plaintext : Orestis - Hacking for fun and profit
Encrypted : Pieagnm - Jkoijeg nbw zwx mle grwsnn

http://rumkin.com/tools/cipher/otp.php

using rumkin, we get the key "fuckmybrain"

http://rumkin.com/tools/cipher/vigenere-keyed.php

Ybgbq wpl gw lto udgnju fcpp, C jybc zfu zrryolqp zfuz xjs rkeqxfrl ojwceec J uovg :)

mnvze://10.10.10.17/8zb5ra10m915218697q1h658wfoq0zc8/frmfycu/sp_ptr

There you go you stupid fuck, I hope you remember your key password because I dont :)

https://10.10.10.17/8ba5aa10e915218697d1c658cdee0bb8/orestis/id_rsa



id_rsa to passphrase

┌──(kali㉿kali)-[~/htb/brainfuck]
└─$ python /usr/share/john/ssh2john.py id_rsa > id_john                             
┌──(kali㉿kali)-[~/htb/brainfuck]
└─$ ls
40939.txt  41006.txt  exploit.html  id_john  id_rsa
┌──(kali㉿kali)-[~/htb/brainfuck]
└─$ subl id_john
┌──(kali㉿kali)-[~/htb/brainfuck]
└─$ john id_john --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 4 OpenMP threads
Note: This format may emit false positives, so it will keep trying even after
finding a possible candidate.
Press 'q' or Ctrl-C to abort, almost any other key for status
3poulakia!       (id_rsa)
Warning: Only 2 candidates left, minimum 4 needed for performance.
1g 0:00:00:04 DONE (2021-06-12 07:39) 0.2212g/s 3172Kp/s 3172Kc/s 3172KC/sa6_123..*7¡Vamos!
Session completed
                     