# Challenges

`nmap $ip -T4 -Pn -p-` - a TCP scan for all 65535 ports.

`nmap $ip -T4 -sU -Pn -p-` - a UDP scan for all 65535 ports.

`sudo nmap -A -sU -p U:111 192.168.0.141` - when performing a UDP port scan, you have to specify the `-sU` and `-p U:111` - for the particular UDP port. or `-sU -p-` for entire 65535 ports.





