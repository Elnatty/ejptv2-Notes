---
description: Discovering Systems or Hosts on a Network
---

# b - Host Discovery with NMAP

### Learn more about Nmap scan types and options

[https://www.exploit-db.com/docs/48097](https://www.exploit-db.com/docs/48097)

### \[Nmap] utility

`sudo nmap -sn 192.168.0.1-254` - sends an ICMP ping request to determine hosts that are up in a network (ping sweep).

### \[netdiscover] utility

`sudo netdiscover -i eth0 -r 192.168.0.0/24` - netdiscover utilises ARP requests to resolve mac address to ip address (to discover hosts in the network).

