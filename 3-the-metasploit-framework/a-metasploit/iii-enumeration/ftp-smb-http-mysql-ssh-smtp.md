---
description: 21, 139/445, 80, 3306, 22, 25/465/587
---

# FTP, SMB, HTTP,MySQL, SSH, SMTP

## **FTP Enumeration**

* FTP (File Transfer Protocol) is a protocol that uses TCP port 21 and is used to facilitate file sharing between a server and client/clients.
* It is also frequently used as a means of transferring files to and from the directory of a web server.
* We can use multiple auxiliary modules to enumerate information as well as perform brute-force attacks on targets running an FTP server.
* FTP authentication utilizes a username and password combination, however, in some cases an improperly configured FTP server can be logged into anonymously.

#### **Practical Demonstration**

```bash
msf6 > use auxiliary/scanner/portscan/tcp #Perform a port scan with an MSF module
msf6 > use auxiliary/scanner/ftp/ftp_version #Enumerate the version of the FTP service
msf6 > use auxiliary/scanner/ftp/ftp_login #Perform a brute force attack against the FTP service to obtain legitimate credentials.
msf6 > use auxiliary/scanner/ftp/anonymous #Check if anonymous login is enabled
```

##

## SMB Enumeration

* SMB (Server Message Block) is a network file sharing protocol that is used to facilitate the sharing of files and peripherals between computers on a local network (LAN).
* SMB uses port 445 (TCP). However, originally, SMB ran on top of NetBIOS using port 139.
* SAMBA is the Linux implementation of SMB, and allows Windows systems to access Linux shares and devices.
* We can utilize auxiliary modules to enumerate the SMB version, shares, users and perform a brute-force attack in order to identify users and passwords.

**Practical Demonstration**

```bash
msf5 > search type:auxiliary name:smb # search query.
msf5 > use 29
msf5 > info # gives info about a particular module.
msf5 > use auxiliary/scanner/smb/smb_version #Perform SMB version detection.
msf5 > use auxiliary/scanner/smb/smb_enumusers #Perform SMB users enumeration.
msf5 > use auxiliary/scanner/smb/smb_enumshares #Perform SMB shares enumeration.
msf5 > use auxiliary/scanner/smb/smb_login #Perform brute-force attack against smb service accounts

smbclient -L \\\\<targetIP>\\ -U <username> #Perform SMB shares enumeration using legitimate credentials
smbclient \\\\<targetIP>\\<share> -U <username> #Interact with a share using legitimate credentials
```



## Web Server Enumeration

* A web server is a software that is used to serve website data on the web.
* Web servers utilize HTTP (HyperText Transfer Protocol) to facilitate the communication between clients and the web server.
* HTTP is an application layer protocol that utilizes TCP port 80 for communication.
* We can utilize auxiliary modules to enumerate the web server version, HTTP headers, brute-force directories and much more.
* Examples of popular web servers are: Apache, Nginx, and Microsoft IIS.

**Practical Demonstration**

```bash
msf5 > use auxiliary/scanner/http/http_version #Perform HTTP version detection
msf5 > use auxiliary/scanner/http/http_header #Perform HTTP header detection
msf5 > use auxiliary/scanner/http/robots_txt #Scan HTTP robots.txt content
msf5 > use auxiliary/scanner/http/dir_scanner #Perform a directory brute-force attack, to find hidden directories.
msf5 > use auxiliary/scanner/http/files_dir # Perform a file brute-force attack.
msf5 > use auxiliary/scanner/http/http_login #Perform a brute-force attack against the HTTP service authentication forms.
set AUTH_URI <path_to_the_secure_dir>
set PASS_FILE and set USER_FILE # then unset USERPASS_FILE
msf5 > use auxiliary/scanner/http/apache_userdir_enum #Returns valid usernames used on the web server.

```



## MySQL Enumeration

* MySQL is an open-source relational database management system based on SQL (Structured Query Language).
* It is typically used to store records, customer data, and is most commonly deployed to store web application data.
* MySQL utilizes TCP port 3306 by default, however, like any service it can be hosted on any open TCP port.
* We can utilize auxiliary modules to enumerate the version of MySQL, perform brute-force attack to identify passwords, execute SQL queries and much more.

**Practical Demonstration**

```bash
msf5 > auxiliary/scanner/mysql/mysql_version #Perform MySQL service version enumeration.
msf5 > auxiliary/scanner/mysql/mysql_login #Perform brute-force attack against MySQL service.
msf5 > auxiliary/admin/mysql/mysql_enum #Perform MySQL enumeration using legitimate credentials
msf5 > auxiliary/admin/mysql/mysql_sql #Allows to perform SQL commands against the SQL service.
MySQL > select version() #MySQL command that returns the version of MySQl
MySQL > show databases; #Returns all databases stored in the MySQl server

msf5 > use auxiliary/scanner/mysql/mysql_schemadump #Extract the schema information from the MySQL server

```



## SSH Enumeration

* SSH (Secure Shell) is a remote administration protocol that offers encryption and is the successor to Telnet.
* It is typically used for remote access to servers and systems.
* SSH uses TCP port 22 by default, however, like other services, it can be configured to use any other open TCP port.
* We can utilize auxiliary modules to enumerate the version of SSH running on the target as well as perform brute-force attacks to identify passwords that can consequently provide us remote access to a target.

**Practical Demonstration**

```bash
msf5 > use auxiliary/scanner/ssh/ssh_version #Perform SSH version detection.
msf5 > use auxiliary/scanner/ssh/ssh_login #Perform brute-force attack against the SSH server (Use ssh_login_pubkey module if SSH is using public key as authentication)
msf5 > use auxiliary/scanner/ssh/ssh_enumusers #Enumerate SSH users
```



## SMTP Enumeration

* SMTP (Simple Mail Transfer Protocol) is a communication protocol that is used for the transmission of email.
* SMTP uses port 25 by default. It can also be configured to run on TCP port 465 and 587.
* We can utilize auxiliary modules to enumerate the version of SMTP as well as user accounts on the target system.

**Practical Demonstration**

```bash
msf5 > use auxiliary/scanner/smtp/smtp_version #Perform SMTP version detection
msf5 > use auxiliary/scanner/smtp/smtp_enum #Perform SMTP user enumeration
```



