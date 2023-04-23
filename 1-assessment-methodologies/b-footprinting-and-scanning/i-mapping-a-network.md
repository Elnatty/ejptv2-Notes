# i - Mapping a Network

## Wireshark

#### Another way to discover hosts in  a network is by using the \[arp-scan] utility

`sudo arp-scan -I eth0 -g 192.168.0.0/24`  scans the entire subnet for hosts that are up.

In wireshark we receive some "arp" requests.

### \[fping] utility

`fping -I eth0 -g 192.168.0.0/24 -a 2>/dev/null` - discovers all host that are up by sending a ping request to them, and thoses that replies are shown.











