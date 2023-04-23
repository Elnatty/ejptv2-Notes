# a - Website Recon & Footprinting

## What are we looking for?

* Ip addresses.
* Directories hidden from search engines.
* Names.
* Email Addresses.
* Phone Numbers.
* Physical Addresses.
* Web Technologies being used.

## Practical Demo

### \[host] utility

`host hackersploit.org` - using the \[host] utility on kali to perform a dns lookup on a target site. Then multiple IPV4 addresses is as a result of the fact that the webserver is behind a firewall/proxy ie "cloudflare".

<figure><img src="../../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

`hackersploit.org/robots.txt` - The robots.txt files allows the dev to specify web files/folder they don't want search engines to index.

`hackersploit.org/sitemap_index.xml` - gives search engines info about pages they can index in searches.

### Browsers Extensions

`builtwith extension` - this gives us detailed info as to all the technologies/CMS of a website.

`wappalyzer extension` - another browser extension for website enumeration.

### \[whatweb] utility

Another builtin tool in kali for footprinting websites. You can specify aggressive or Setalth scan mode, UserAgent option, User Cookie authentication etc.

`whatweb hackersploit.com` - for a basic enumeration.

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### Downloading an Entire Website

If you want to analyse the source code of a website, you can download the entire website with a tool called `HTTrack` . `sudo apt-get install webhttrack` - to download for linux.

`webhttrack` - launch a webconsole UI running on port 8080.









