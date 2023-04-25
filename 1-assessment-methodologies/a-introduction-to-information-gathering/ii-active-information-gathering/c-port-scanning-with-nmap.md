---
description: identifying open ports and services running on these open ports.
---

# c - Port Scanning with NMAP

`nmap 192.168.0.141` - performs a SYN scan on the 1st 1000 ports.

`nmap -Pn 192.168.0.141` - we specify the `-Pn` which disables ping(ICMP), because by default Windows machines disables ping request by defualt.

<figure><img src="../../../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

`nmap -Pn -p- 192.168.0.141` - entire 65535 ports scan.

`nmap -Pn -F 192.168.0.141` - 1st 100 ports scan.

`nmap -Pn -sU 192.168.0.141` - UDP scans.

`nmap -Pn -F 192.168.0.141 -v` - display output during the scan (-v).

`nmap -Pn -F -sV 192.168.0.141` - service versions detection scan (-sV).

`sudo nmap -Pn -F -sV -O 192.168.0.141` - OS detection scan (-O).

`sudo nmap -Pn -F -sV -O -sC 192.168.0.141 -v` - default script scan (-sC).

`sudo nmap -Pn -F -A 192.168.0.141 -v`  - Aggressive scan (-A) combines the (-sV, -O, -sC) into one.

`sudo nmap -Pn -F -A -T4 192.168.0.141 -v` - TImer templates (T1-T5),

* T0 - paranoid.
* T1 - sneaky.
* T2 - polite.
* T3 - normal.
* T4 - aggressive.
* T5 - insane

`sudo nmap -Pn -F -A 192.168.0.141 -v -oN` - outputing results to a .txt file (-oN).

`sudo nmap -Pn -F -A 192.168.0.141 -v -oX` - outputing results to a .xml file (-oX), this output can be imported into a framework like Metasploit.

