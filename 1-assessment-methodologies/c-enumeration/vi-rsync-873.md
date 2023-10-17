# vi -Rsync - 873

Rsync is a utility for transferring and synchronizing files between two servers (usually Linux).

[https://infra.newerasec.com/infrastructure-testing/enumeration/services-ports/873-rsync](https://infra.newerasec.com/infrastructure-testing/enumeration/services-ports/873-rsync)

[https://book.hacktricks.xyz/network-services-pentesting/873-pentesting-rsync](https://book.hacktricks.xyz/network-services-pentesting/873-pentesting-rsync)

{% code overflow="wrap" lineNumbers="true" %}
```bash
nmap -p 873 192.168.0.1

PORT    STATE SERVICE REASON
873/tcp open  rsync   syn-ack

# using nc
nc -vn 127.0.0.1 873
```
{% endcode %}

### Mannual Enumeration

{% code overflow="wrap" lineNumbers="true" %}
```bash
# List directory
rsync 192.168.1.171::

# list directory ipv6
rsync --list-only -a rsync://[dead:beef::0250:56ff:fe88:e5fa]:8730/var/

# save all victim files in a folder user_files.
rsync -av rsync://rsync-connect@10.10.172.210/files user_files
```
{% endcode %}



