# i - Installing & Configuring The Metasploit Framework

### **Installing The Metasploit Framework**

* The MSF is distributed by Rapid7 and can be downloaded and installed as a standalone package on both Windows & Linux.
* In this course we will be utilizing the Metasploit Framework on Linux and our preferred distribution of choice is Kali Linux.
* MSF and it's required dependencies come pre-packaged with Kali Linux which saves us from the tedious process of installing MSF manually.

### **The Metasploit Framework Database**

* The Metasploit Framework Database (msfdb) is an integral part of the Metasploit Framework ans is used to keep track of all your assessments, host data scans, etc.
* The Metasploit Framework uses PostgreSQL as the primary database server, as a result, we will also need to ensure that the PostgreSQL database service is running and configured correctly.
* The Metasploit Framework Database also facilitates the importation and storage of scan results from various third party tools like Nmap and Nessus.

### **Installation Steps**

* Update our repositories and upgrade our Metasploit Framework to the latest version.
* Start and enable the PostgreSQL database service.
* Initialize the Metasploit Framework Database (msfdb)
* Launch MSFconsole!



## **Practical Demonstration**

```bash
# Using Kali Linux
sudo apt-get update && sudo apt-get install metasploit-framework -y # Update repositories and install msf

sudo systemctl enable postgresql #Enable the postgreSQL service
sudo systemctl start postgresql #Start the postgreSQL service
sudo systemctl status postgresql #Check the status of the postgreSQL service

sudo msfdb init #Initialize the msfdb
sudo msfdb reinit #Reinitialize the msfdb (Delete and reset the DB)
sudo msfdb status #Check the status of the msfdb

msfconsole #Launch the msfconsole

msf6 > db_status #Check the status of the msfdb
```

## MSFconsole Fundamentals

### **MSFconsole Fundamentals**

* Before we can start using the Metasploit Framework for penetration testing, we need to get an understanding of how to use MSFconsole.
* The Metasploit Framework Console (MSFconsole) is an easy-to-use all in one interface that provides you with access to all the functionality of the Metasploit Framework.
* We will be utilizing MSFconsole as our primary MSF interface for the rest of the course.

### **What You Need To Know**

1. \- How to search for modules.
2. \- How to select modules.
3. \- How to configure module options & variables.
4. \- How to search for payloads.
5. \- Managing sessions.
6. \- Additional functionality.
7. \- Saving your configuration.

### **MSF Module Variables**

* MSF modules will typically require information like the target & host IP address and port in order to initiate a remote exploit/connection.
* These options can be configured through the use of MSF variables.
* MSFconsole allows you to set both local variable values or global variable values.

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

## **Practical Demonstration**

```bash
msf6 > help #Show the help page
msf6 > version #Returns the version of MSF
msf6 > show all #Returns all available modules
msf6 > show exploits #Returns all available exploit (also works with auxiliary, scanners, etc...)
msf6 > show -h #Show the help page for the "show" command, works with a lot of commands

msf6 > search portscan #Returns a list of modules corresponding the search (portscan in this example)
msf6 > use <modulePath> #Load a module
msf6 > show options #Returns the list of variables that you can/have to configure in order for the module to work as intended
msf6 > set <variable> <value> #Set a value to a variable
msf6 > setg <variable> <value> #Set a global value to a variable
msf6 > run #Start the module, you can also use "exploit"
msf6 > back #Unselect a module

msf6 > set payload <payloadPath> #Change the payload for the selected exploit

msf6 > sessions #List all active sessions

msf6 > connect <targetIP> <port> #Similar to netcat, allow to grab banners
```

## Creating & Managing Workspaces

### **MSF Workspaces**

* Workspaces allow you to keep track of all your hosts, scans and activities and are extremely useful when conducting penetration tests as they allow you to sort and organize your data based on the target or organization.
* MSFconsole provides you with the ability to create, manage and switch between multiple workspaces depending on your requirements.
* We will be using work spaces to organize our assessments as we progress through the course.

## **Practical Demonstration**

```bash
msf6 > workspace -h #Show the workspace help page
msf6 > workspace #List workspaces
msf6 > workspace -a <name> #Create a new workspace
msf6 > workspace <name> # switch to another workspace
msf6 > workspace -d <name> #Delete a workspace
msf6 > workspace -r <oldName> <newName> #Rename a workspace

#Relative to the current workspace
msf6 > hosts #List every discovered hosts
msf6 > services #List every services discovered for every hosts
msf6 > creds #List every credentials discovered
msf6 > loot #List every interesting discoveries
msf6 > notes #List every notes from post exploitation modules
```



## How to Search for anything in Metasploit

```bash
# easily search for anything in metasploit
# examples

search -h

# method 1
search type:exploit platform:windows smb # restrict searches to all smb exploits for windows only.
search type:exploit platform:linux ftp # all ftp exploits for linux only

# method 2
# what grep does is: get the particular keyword "auxiliary" from the search result "erernalblue"
grep auxiliary search eternalblue 
```



