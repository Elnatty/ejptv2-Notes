# iv - HTTP IIS - 80

`nmap $ip -p80 -sV -O` - more scan.

### \[whatweb] utility

`whatweb $ip` - enumerates more info and gives us insight as to what technologies the site was built with.

### \[http] utility

`http $ip` - gives us server header response information.

### \[dirb] utility

`dirb http://$ip` - searches for hidden directories.

`dirb http://$ip /usr/share/metasploit-framework/wordlists/directory.txt` - specifying a wordlist.

## HTTP Nmap enumeration

`nmap -$ip -p80  -sV --script http-enum` - returns directories, and services running.

`nmap -$ip -p80  -sV --script http-headers` - enumerate http-headers.

`nmap -$ip -p80  -sV --script http-methods --script-args http-methods.url-path=/webdav/` -  enumerates the supported http-methods.

`nmap --script http-methods --script-args http-methods.url-path=/webdav/ $ip` - scanning a directory for supported methods.

### Metasploit

```bash
# 1
use auxiliary/scanner/http/http_version
set RHOSTS $ip
options
run

# 2
use auxiliary/scanner/http/brute-dirs
show options
set RHOSTS $ip
run

# 3
# robots.txt
use auxiliary/scanner/http/robots.txt
set rhosts $ip
options
```

### \[curl ] utility

`curl $ip` - get entire webpage, then get important information from the retrieved content.

### \[wget] utility

`wget "http://$ip/index"` - download a webpage.

### \[browsh] utility

`browsh --startup-url $ip` - renders in the cmd line a GUI representation of the webpage.

### \[lynx] utility

`lynx http://$ip` - a similar GUI representation of the webpage in the cmd line.



