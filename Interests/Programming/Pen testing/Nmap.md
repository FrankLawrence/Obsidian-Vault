port 1-65535 all

/usr/share/nmap/scripts

-T4 : speed of nmap more prone to errors

-A : find the type of OS used

-sV : find service type (important)

-sC : default scripts

nmap -p 1-65535 -T4 -A -sV 192.168.1.109 2>&1 | tee /var/tmp/scan.txt

nmap <IP> -p  -T$<port> —script=<script> —scripts-args=<script.cmd>='<command>'