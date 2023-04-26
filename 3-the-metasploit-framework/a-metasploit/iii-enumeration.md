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

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

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

curl <targetIP> #Download the webpage (if the host is hosting one).

msf5 > run autoroute -s <subnetTargetIP> #Route the traffic to the subnet's target IP address of the first target previously exploited
```

























