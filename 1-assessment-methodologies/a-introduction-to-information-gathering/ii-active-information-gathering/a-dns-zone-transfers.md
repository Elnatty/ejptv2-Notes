# a - DNS Zone Transfers

### DNS Records

* A - resolves a hostname or domain to an ipv4 address.
* AAAA - resolves a hostname or domain to an ipv6 address.
* NS - references to the domain nameservers.
* MX - resolves a domain to a mail server.
* CNAME - used for domain aliases.
* TXT - text records.
* HINFO - host information.
* SOA - domain authority.
* SVR - service records,
* PTR - resolves an ip address to a hostname.

## Zone Transfer Definition

In certain cases DNS server admins may want to copy or transfer zone files from one DNS server to another. This process is called zone transfer.

### \[dig], \[dnsrecon] and \[dnsenum] utility

`dnsenum zonetransfer.me` - outputs full dns records.

`dig axfr @nsztm1.digi.ninja zonetransfer.me` - specifies the entire zone records for the specified domain name server, where \``@nsztm1.digi.ninja`  is the primary name server for the zonetransfer.me domain.

`dnsrecon -d zonetransfer.me -t axfr` - outputs full dns zone records.

`fierce --domain aztecmfbank.com` - another tool.

