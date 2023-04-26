# ii - Nmap

### **Port Scanning & Enumeration With Nmap**

* Nmap is a free and open-source network scanner that can be used to discover hosts on a network as well as scan targets for open ports.
* It can also be used to enumerate the services running on open ports as well as the operating system running on the target system.
* We can output the results of our Nmap scan in to a format that can be imported into MSF for vulnerability detection and exploitation.

## **Practical Demonstration**

```bash
nmap #Show the nmap help page
nmap <targetIP> #Perform a default scan
nmap <...> -sV #Perform service enumeration
nmap <...> -O #Perform OS detection
nmap <...> -oX <fileName> #Output the scan as an XML file for importing to Metaasploit Framework.

```

## Importing Nmap Scan Results Into MSF

```bash
# create a workspace for the nmap scan results you want to work on.
# db_import /home/dking/metasploitable_scan
msf5 > db_import <pathToFile> #Import a file to MSF
msf5 > services #Show the open ports and correspondant services of discovered hosts in MSF
msf5 > db_nmap <...> #Perform an nmap scan within MSF and saves it directly in the msfdb
msf5 > vulns #List out vulnerabilities detected within the hosts in the msfdb
```



