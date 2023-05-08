---
description: Linux and Windows
---

# Privilege Escalation Checklists

## <mark style="color:red;">Linux Privilege Escalation checklist:</mark>

[https://infosecwriteups.com/linux-privilege-escalation-linux-kernel-distribution-exploits-you-should-now-about-1c46152d133d](https://infosecwriteups.com/linux-privilege-escalation-linux-kernel-distribution-exploits-you-should-now-about-1c46152d133d)

<details>

<summary>Checklist for Linux</summary>



### Kernel, OS & Device Information <a href="#c15a" id="c15a"></a>

{% code overflow="wrap" lineNumbers="true" %}
```bash
#Can the current user perform anything as root
sudo -l#Print all available system information
uname -a#Kernel release
uname -r#System hostname
uname -n
hostname#Linux Kernel Architecture (32 or 64 bit)
uname -m#Kernel information
cat /proc/version#Distribution information
cat /etc/*-release
cat /etc/issue#CPU information
cat /proc/cpuinfo#File system information
df -a
```
{% endcode %}

### Users & Groups <a href="#3c88" id="3c88"></a>

{% code overflow="wrap" lineNumbers="true" %}
```bash
#List all users on the system
cat /etc/passwd#List all groups on the system
cat /etc/group#List all UID’s and respective memberships
for i in $(cat /etc/passwd 2>/dev/null| cut -d”:” -f1 2>/dev/null);do id $i;done 2>/dev/null#Show user hashes — (Privileged command)
cat /etc/shadow#List all super user accounts
grep -v -E “^#” /etc/passwd | awk -F: ‘$3 == 0 { print $1}’#Users currently logged in
finger
pinky
users
who -a#Who is currently logged in and what they are doing
w#Listing of last logged on users
last#Information on when all users last logged in
lastlog#Information on when the specified user last logged in
lastlog -u <username>#Entire list of previously logged on users
lastlog | grep -v “Never”
```
{% endcode %}

### User & Privilege Information <a href="#60b8" id="60b8"></a>

{% code overflow="wrap" lineNumbers="true" %}
```bash
#Current username
whoami#Current user information
id#Who is allowed to do what as root — Privileged command
cat /etc/sudoers#Can the current user perform anything as root
sudo -l#Can the current user run any ‘interesting’ binaries as root and if so also display the binary permissions etc.
sudo -l 2>/dev/null | grep -w ‘nmap|perl|’awk’|’find’|’bash’|’sh’|’man’|’more’|’less’|’vi’|’vim’|’nc’|’netcat’|python |ruby|lua|irb’ | xargs -r ls -la 2>/dev/null
```
{% endcode %}

### Environment Information <a href="#97f9" id="97f9"></a>

```bash
#Display environmental variables
env
set#Path information
echo $PATH#Displays command history of current user
history#Print working directory, that is, where am I
pwd#Display default system variables
cat /etc/profiles#Display available shells
cat /etc/shells
```

### Service Information <a href="#6138" id="6138"></a>

{% code overflow="wrap" %}
```bash
#View services running as root
ps aux | grep root#Lookup process binary path and permissions
ps aux | awk ‘{print $11}’|xargs -r ls -la 2>/dev/null |awk ‘!x[$0]++’#List services managed by inetd
cat /etc/inetd.conf#As above for xinetd
cat /etc/xinetd.conf#A very ‘rough’ command to extract associated binaries from xinetd.conf and show permissions of each
cat /etc/xinetd.conf 2>/dev/null | awk ‘{print $7}’ |xargs -r ls -la 2>/dev/null#Permissions and contents of /etc/exports (NFS)
ls -la /etc/exports 2>/dev/null; cat /etc/exports 2>/dev/null
```
{% endcode %}

### Jobs/Tasks <a href="#9d2e" id="9d2e"></a>

{% code overflow="wrap" %}
```bash
#Display scheduled jobs for the specified user — Privileged comamand
crontab -l -u <username>#Scheduled jobs overview
ls -la /etc/cron*#What can ‘others’ write in /etc/cron* directories
ls -aRl /etc/cron* | awk ‘$1 ~ /w.$/’ 2>/dev/null#List of current tasks
top
```
{% endcode %}

### Networking, Routing & Communications <a href="#4ee8" id="4ee8"></a>

{% code overflow="wrap" %}
```bash
#List of network interfaces/sbin/ifconfig -a#As above
cat /etc/network/interfaces#Display ARP communications
arp -a#Display route information
route#Display routing table entry. Also to find-out router’s IP address.
ip route #Show configured DNS server addresses
cat /etc/resolv.conf#List all TCP sockets and related PIDs (-p Privileged command)
netstat -antp#List all TCP sockets and related PIDs (-p Privileged command)
netstat -anup#List rules — Privileged command
iptables -L#View port numbers/services mappings
cat /etc/services
```
{% endcode %}

### Programs Installed <a href="#6a81" id="6a81"></a>

{% code overflow="wrap" %}
```bash
#Installed packages (Debian)
dpkg -l#Installed packages (Red Hat)
rpm -qa#sudo version — does an exploit exist?
sudo -V#Apache version
httpd -v
apache2 -v#List loaded Apache modules
apache2ctl (or apachectl) -M#Installed MYSQL version details
mysql — version#Installed Postgres version details
psql -V#Installed Perl version details
perl -v#Installed Java version details
java -version#Installed Python version details
python — version#Installed Ruby version details
ruby -v#Locate ‘useful’ programs (netcat, wget etc)
find / -name %program_name% 2>/dev/null
(i.e. nc, netcat, wget, nmap etc)
which %program_name% (i.e. nc, netcat, wget, nmap etc)#List available compilers
dpkg — list 2>/dev/null| grep compiler |grep -v decompiler 2>/dev/null && yum list installed ‘gcc*’ 2>/dev/null| grep gcc 2>/dev/null#Which account is Apache running as
cat /etc/apache2/envvars 2>/dev/null |grep -i ‘user|group’ |awk ‘{sub(/.*export /,””)}1’#Check installed applications, if found search for their exploit
cd /var; ls
```
{% endcode %}

### Search for interesting files <a href="#6b43" id="6b43"></a>

{% code overflow="wrap" %}
```bash
#Find SUID files
find / -perm -4000 -type f 2>/dev/null#Find SUID files owned by root
find / -uid 0 -perm -4000 -type f 2>/dev/null#Find GUID files
find / -perm -2000 -type f 2>/dev/null#Find world-writable files
find / -perm -2 -type f 2>/dev/null#Find world-writable files excluding those in /proc
find / ! -path “*/proc/*” -perm -2 -type f -print 2>/dev/null#Find world-writable directories
find / -perm -2 -type d 2>/dev/null#Find rhost config files
find /home –name *.rhosts -print 2>/dev/null#Find *.plan files, list permissions and cat the file contents
find /home -iname *.plan -exec ls -la {} ; -exec cat {} 2>/dev/null ;#Find hosts.equiv, list permissions and cat the file contents
find /etc -iname hosts.equiv -exec ls -la {} 2>/dev/null ; -exec cat {} 2>/dev/null ;#Check if you can access other user directories to find interesting files
ls -ahlR /root/#Show the current user’s command history
cat ~/.bash_history#Show current user’s various history files
ls -la ~/.*_history#Can we read root’s history files
ls -la /root/.*_history#Check for interesting ssh files in the current user’s directory
ls -la ~/.ssh/#Find SSH keys/host information
find / -name “id_dsa*” -o -name “id_rsa*” -o -name “known_hosts” -o -name “authorized_hosts” -o -name “authorized_keys” 2>/dev/null |xargs -r ls -la#Check Configuration of inetd services
ls -la /usr/sbin/in.*#Check log files for keywords (‘pass’ in this example) and show positive matches
grep -l -i pass /var/log/*.log 2>/dev/null#List files in specified directory (/var/log)
find /var/log -type f -exec ls -la {} ; 2>/dev/null#List .log files in specified directory (/var/log)
find /var/log -name *.log -type f -exec ls -la {} ; 2>/dev/null#List .conf files in /etc (recursive 1 level)
find /etc/ -maxdepth 1 -name *.conf -type f -exec ls -la {} ; 2>/dev/null
ls -la /etc/*.conf#Find .conf files (recursive 4 levels) and output the number where the word ‘password’ is located
find /etc/ -maxdepth 1 -name *.conf -type f -exec ls -la {} ; 2>/dev/null#List open files (output will depend on account privileges)
lsof -I -n#Can we read roots mail
head /var/mail/root#To identify the binary capability files with the help of getcap. Source: 
getcap -r / 2>/dev/null
```
{% endcode %}



</details>

<details>

<summary>Linux Priv Esc Mind-map</summary>

![](<.gitbook/assets/image (9).png>)

</details>

1. ### Kernel vulnerabilities:
   * `uname -a`: displays the kernel version and architecture.
   * `dmesg`: displays kernel messages and can reveal information about kernel vulnerabilities.
   * `systemctl status`: displays the status of systemd services and can reveal information about running kernel modules.
2.  ### SUID/SGID binaries:

    * `find / -perm -4000 2>/dev/null`: finds SUID binaries.
    * `find / -perm -2000 2>/dev/null`: finds SGID binaries.
    * `ls -l /usr/bin/sudo`: checks if the `sudo` binary has the SUID bit set.

    In case you find `/bin/systemctl` binary, you can create a service and execute it using the `systemctl enable <serviceName.service>` to  enable the service, then `systemctl start <servicename.service>`  to start the service, you should get a root shell.... check this blog for more info.. [https://medium.com/@klockw3rk/privilege-escalation-leveraging-misconfigured-systemctl-permissions-bc62b0b28d49](https://medium.com/@klockw3rk/privilege-escalation-leveraging-misconfigured-systemctl-permissions-bc62b0b28d49)
3. ### Writable directories and files:
   * `find / -writable -type d 2>/dev/null`: finds writable directories.
   * `find / -writable -type f 2>/dev/null`: finds writable files.
   * `ls -la /etc/passwd`: checks if the `/etc/passwd` file is writable.
4. ### Processes with elevated privileges:
   * `ps aux --forest`: displays a hierarchical view of running processes.
   * `top`: displays real-time information about running processes.
   * `cat /proc/*/status | grep -E "Uid|Gid"`: displays the UIDs and GIDs of all running processes.
5. ### Installed software and services:
   * `dpkg -l`: lists all installed packages on Debian-based systems.
   * `rpm -qa`: lists all installed packages on Red Hat-based systems.
   * `systemctl list-unit-files --type=service`: lists all installed services using systemd.
6. ### Open ports and network services:
   * `netstat -tulnp`: displays all listening ports and associated processes.
   * `nmap -sS -p- -T4 <target>`: scans all TCP ports on a target using the `nmap` tool.
7. ### Cron jobs and scheduled tasks:
   * `crontab -l`: lists the current user's cron jobs.
   * `ls -la /etc/cron*`: lists system-wide cron jobs.
   * `systemctl list-timers`: lists systemd timers and scheduled tasks.

## <mark style="color:red;">Windows Privilege Escalation Enumeration Commands:</mark>

{% embed url="https://github.com/xXxhagenxXx/OSCP_Cheat_sheet/blob/main/Windows%20Privilege%20Escalation%20Techniques.md" %}
allinone checklist
{% endembed %}

[https://medium.com/@sushantkamble/windows-privilege-escalation-without-metasploit-9bad5fbb5666](https://medium.com/@sushantkamble/windows-privilege-escalation-without-metasploit-9bad5fbb5666)

<details>

<summary>Windows Priv Esc Mind map</summary>

![](<.gitbook/assets/image (20).png>)

</details>

* Stored Credentials
* Windows Kernel Exploiytation
* DLL Hijacking
* Unquoted Service Path
* Weak Folder Permissions
* Weak Service Permission
* Weak Registry Permission
* Always Install Elevated
* Modifiable Autorun
* Token Impersonation

1. ### Kernel vulnerabilities:
   * `systeminfo`: displays system information, including the OS version and build number.
   * `driverquery`: displays information about installed drivers.
   * `wmic qfe get hotfixid`: lists installed Windows updates.
2. ### Services running with elevated privileges:
   * `sc query`: lists all running services and their statuses.
   * `tasklist /SVC`: lists all running processes and associated services.
3. ### Weak file system permissions:
   * `icacls <filename>`: displays the ACLs for a file or directory.
   * `accesschk.exe -wvu "C:\Program Files\"`: searches for directories with weak permissions.
4. ### Registry keys with weak permissions:
   * `reg query HKLM /f "keyword" /t REG_SZ /s`: searches for registry keys containing a keyword.
   * `reg query HKLM /f "keyword" /t REG_BINARY /s`: searches for binary registry values containing a keyword.
5. ### Installed software and services:
   * `wmic product get name`: lists all installed programs.
   * `sc query type= service`: lists all installed services.
6. ### Open ports and network services:
   * `netstat -ano`: displays all listening ports and associated processes.
   * `nmap -sT -p- -T4 <target>`: scans all TCP ports on a target using the `nmap` tool.
7. ### Scheduled tasks and startup programs:
   * `schtasks /query /fo LIST /v`: lists all scheduled tasks.
   * `tasklist /SVC` - running processes.
   * `net start`&#x20;
   * `DRIVERQUERY` - Installed drivers (Some 3rd party drivers
   * `reg query HKCU\Software\Microsoft\Windows\CurrentVersion\Run`: lists all startup programs for the current user.
   * `reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Run`: lists all startup programs for all users.

<details>

<summary>Search for clear-text Passwords in Windows</summary>



{% code overflow="wrap" lineNumbers="true" %}
```bash
find /I password *.txt
find /I password *.xml
find /I password *.ini#Find all these strings in config files
dir /s *pass* == *cred* == *vnc* == *.config*#Find password string in all files.
findstr /spin “password” *.*
findstr /spin “password” *.*


# These are common files to find passwords. They might be base64-encoded. So look out for that.
c:\sysprep.inf
c:\sysprep\sysprep.xml
c:\unattend.xml
%WINDIR%\Panther\Unattend\Unattended.xml
%WINDIR%\Panther\Unattended.xml

dir c:\*vnc.ini /s /b
dir c:\*ultravnc.ini /s /b
dir c:\ /s /b | findstr /si *vnc.ini


# Search for passwords in Registry
VNC
reg query "HKCU\Software\ORL\WinVNC3\Password"

# Windows autologin
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon"
```
{% endcode %}

</details>



