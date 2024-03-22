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
- TCP SYN ping : `nmap -sn -PS 10.10.x.x`
- TCP SYN ping with ports : `nmap -sn -PS1-1000 10.10.x.x`
  Quicker, but only shows if hosts are up or not
- First `nmap -sn -v -T4 10.10.x.x`
- second with port findings `nmap -sn -PS21,22,80,445`

- Zone transfer with: `dnsenum google.com`: https://ankisinha.medium.com/dnsenum-a-command-line-information-gathering-tool-a535078207a6 , `$ fierce --domain google.com --subdomains accounts admin ads` : https://github.com/mschwager/fierce
