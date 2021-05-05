kali@kali:~/oscptraining/tryhackme/hackpack$ ps -fA | grep python
root         581       1  0 14:50 ?        00:00:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-for-signal
kali        1632     797  0 14:59 ?        00:00:00 /usr/bin/python3 /usr/bin/blueman-applet
kali        1726       1  0 14:59 ?        00:00:00 /usr/bin/python3 /usr/bin/blueman-tray
root        5526    5435  0 19:06 pts/4    00:00:00 sudo python3 -m http.server 80
root        5527    5526  0 19:06 pts/4    00:00:03 python3 -m http.server 80
kali        7340    5435  0 22:00 pts/4    00:00:00 grep --color=auto python
kali@kali:~/oscptraining/tryhackme/hackpack$ sudo kill -9 5526
[1]+  Killed                  sudo python3 -m http.server 80  (wd: ~)
(wd now: ~/oscptraining/tryhackme/hackpack)
kali@kali:~/oscptraining/tryhackme/hackpack$ sudo kill -9 5527
kill: (5527): No such process
kali@kali:~/oscptraining/tryhackme/hackpack$ ps -fA | grep python
root         581       1  0 14:50 ?        00:00:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-for-signal
kali        1632     797  0 14:59 ?        00:00:00 /usr/bin/python3 /usr/bin/blueman-applet
kali        1726       1  0 14:59 ?        00:00:00 /usr/bin/python3 /usr/bin/blueman-tray
kali        7350    5435  0 22:00 pts/4    00:00:00 grep --color=auto python

host webserver
sudo python3 -m http.server 80
