# Attack plan...

## 1 Information Gathering

![alt text](/assets/pentesting_flow.png)
Quick notes

### 1.1 passive infogathering

Looking for:

- IP addresses
- DNS records
- web technologies
- OS

use:

- `whatweb <dns>`
- sitemap.xml
- robots.txt
- firefox addon: build with,wappalyzer
- who.is lookup, or terminal: `whois <DNS>`
- `dnsrecon -d microsoft.com`
- dnsdumbster.com
- detect firewall: `wafw00f < dns > -a`
- harvest emails: `theHarvester -d DOMAIN -b all` ++

### 1.2 Active Information Gathering

- nmap scan for tcp: `nmap -p- -Pn -v -sV -sC <ip>`
- nmap scan for udp: `nmap -sU -T4  <ip>`

- Zone transfer with: `dnsenum google.com`: https://ankisinha.medium.com/dnsenum-a-command-line-information-gathering-tool-a535078207a6 , `$ fierce --domain google.com --subdomains accounts admin ads` : https://github.com/mschwager/fierce
