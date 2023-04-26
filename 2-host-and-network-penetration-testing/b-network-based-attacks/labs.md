# Labs

## Tshark

**Tshark is the command line version of Wireshark**

```bash
tshark -v #Returns the version of Tshark
tshark -h #Returns the help menu
tshark -r <PCAPfile> #Display the results of a .pcap file
tshark -r <PCAPfile> -z io,phs -q #Return statistics about frames and protocols hierarchy

# Filtering Basics
tshark -r HTTP_traffic.pcap -Y 'http' #Filter results to get only traffic of HTTP service (can work with other services as well)
tshark -r HTTP_traffic.pcap -Y 'ip.src==<sourceIP> && ip.dst==<destinationIP>' #Filter result to get only traffic bewteen two IPs
tshark -r HTTP_traffic.pcap -Y 'http.request.method==GET' # all GET requests.
tshark -r HTTP_traffic.pcap -Y 'http.request.method==GET' -Tfields -e frame.time -e ip.src -e http.request.full_uri # filter by time, src ip, and url.


# ARP Poisoning.
# arpspoof -i <interface> -t <targetIP> -r <localIP> #Perform an ARP spoofing attack


```







