# Information Gathering techniques

2 types of information gathering.
first phase og pentest.

1.  passive = not activly engaging the target. public available resources.
2.  active = activly engaging the target. port scans, will require authorization!!

## what information are we looking for?

    Passsive:
        - IP addreses & DNS information
        - Domain names and domain ownership information
        - Email addresses and social media profiles
        - Web technologies being used on target sites
        - Identifying subdomains
    Active:
        - Discovering open ports on target systems
        - Learning about internal infrastructure of a target network/organisation
        - enumeration information from target systems

## Passive

### Passive: Website Recon & Footprinting

    Optain IP address: terminal -> host <DNS * hackersploit.org or google.com etc>

    Check:

- < DNS >/robots.txt
- sitemap.xml - check sitemap categories for hidden folders not showing on the frontend

Add-on on firefox:

- built with
- wappalyzer
  Terminal: whatweb < DNS >

### Passive: Whois lookup

    command line: whois < DNS >
    website: who.is

    Can be performed with an IP address

### Passive: Netcraft

    gather information on a target domain.
    go to page: services -> data mining

### Passive: DNS Recon

https://medium.com/@techmindxperts/what-is-dnsrecon-full-guide-with-examples-355aba308332

- python script
- pre-packaged with kali
- terminal tool

### Passive: DNS Dumbster

- site: dnsdumbster.com
- can find subdomains under "Hosts"
- gives graph and mapping

### Passive: WAF detection, wafw00f

    detecting firewalls on a website

- github: https://github.com/EnableSecurity/wafw00f

- pre-packaged with kali

- cmd: wafw00f < dns > -a

### Passive: subdomain enumeration, sublist3r

    https://github.com/aboul3la/Sublist3r

    - might need to use VPN after awhile, sends bunch of requests might hit rate limit on search engines.

### Passive: Google Dorking

Google Dorking cheatsheet: https://gist.github.com/sundowndev/283efaddbcf896ab405488330d1bbc06

- limit all results to domain: site:< test.com >
- spesific results: site:< test.com > inurl:< your ask >
- subdomains: site:\*< test.com >
- title: site:\*< test.com > intitle:admin
- find filetypes: filetype:xlsx
- find passswords?: inurl:auth_user_file.txt , inurl:passwd.txt

_Google dorking people_
https://www.maltego.com/blog/using-google-dorks-to-support-your-open-source-intelligence-investigations/

- if you only have username(using example username) and want to find email addresses: BadGuy1\*.com
- documents relevant to target: “John J. Doe” filetype:pdf OR filetype:xlsx OR filetype:docx

### Passive: email harvesting with Harvester

    https://github.com/laramies/theHarvester

- NB! getting some invalid soruce error
- use `theHarvester -d DOMAIN -b all`

## Active

Active information gathering needs to be accepted by the site you want to scan.

### Active: DNS Zone Transfers, DNS Interrogation

- DNS = Domain Name System, protocol to resolve domane names/hostname to IP addresses
- DNS Interrogation is the proccess of enumerationg DNS records for a sepcific domain
- DNS Zone Transfers is transfering or copying zone files from one DNS server to another.
- zone files = files that contain information about a domain
- If misconfigured can lead to abuse from attackers to download zone files.
- Tools dns zone transfer: dnsenum, dig, fierce

### Nmap Host discovery and port scanning

- Host Discovery: do a ping sweap,ping scan. use flag _-sn_ for no port scan.
- use netdiscover for better GUI

---

`

- Port Scanning
  `-Pn` to disable ping scan
  `-p-` all ports
  `-sU` UDP ports
  `-v` verbose scan
  `-sV` service version scan flag
  `-F` fast scan
  `-O` trying to find OS, might not be accurate
  `sC` running nmap scripts
  `-T` from 0 -> 5, sneaky to insane `-T2`
  `-oN outputnmap.txt` output to text file
  `-oX outputnmap.xml` xml format
- _filtered_ might means that port might have a firewall or closed

nmap scan for tcp: `nmap -p- -Pn -v -sV -sC <ip>`
nmap scan for udp: `nmap -sU -T4  <ip>`
