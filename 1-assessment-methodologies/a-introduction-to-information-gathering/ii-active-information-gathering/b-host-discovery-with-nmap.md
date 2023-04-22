---
description: Discovering Systems or Hosts on a Network
---

# b - Host Discovery with NMAP

### \[Nmap] utility

`sudo nmap -sn 192.168.0.0/24` - sends an ICMP ping request to determine hosts that are up in a network.

### \[netdiscover] utility

`sudo netdiscover -i eth0 -r 192.168.0.0/24` - netdiscover utilises ARP requests to resolve mac address to ip address (to discover hosts in the network).

