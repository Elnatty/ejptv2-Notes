# Labs

## Tshark

**Tshark is the command line version of Wireshark**

{% code overflow="wrap" lineNumbers="true" %}
```bash
tshark -v #Returns the version of Tshark
tshark -h #Returns the help menu
tshark -r <PCAPfile> #Display the results of a .pcap file
tshark -r <PCAPfile> -z io,phs -q #Return statistics about frames and protocols hierarchy

# Filtering Basics

# getting the OS (operating system) info of a src ip addr
tshark -r <pcap_file> -Y "ip.src==192.168.1.1 && http" -Tfields -e http.user_agent

tshark -r HTTP_traffic.pcap -Y 'http' #Filter results to get only traffic of HTTP service (can work with other services as well)
tshark -r HTTP_traffic.pcap -Y 'ip.src==<sourceIP> && ip.dst==<destinationIP>' #Filter result to get only traffic bewteen two IPs
tshark -r HTTP_traffic.pcap -Y 'http.request.method==GET' # all GET requests.
tshark -r HTTP_traffic.pcap -Y 'http.request.method==GET' -Tfields -e frame.time -e ip.src -e http.request.full_uri # filter by time, src ip, and url.

# display 1st 100 packets
tshark -r HTTP_traffic.pcap -c 100 

# list all interfaces supported by tshark
tshark -D

# sniff on a particular interface
tshark -i



# ARP Poisoning.
# arpspoof -i <interface> -t <targetIP> -r <localIP> #Perform an ARP spoofing attack


```
{% endcode %}







