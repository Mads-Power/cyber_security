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

- common command nmap: `nmap -Pn -sS -F -T4 <ip>`
- nmap scan for tcp: `nmap -p- -Pn -v -sV -sC -T4 <ip>`

- ping scan `nmap -sn 192.170.60.0/24`
- common command nmap: `nmap -Pn -sS -F -T4 <ip>`
- 1. `nmap -Pn <ip>`
- 2. run OS, Version, script and traceroute detection in one `-A` but it is intrusive.
- 3. nmap scan for tcp: `nmap -p- -Pn -v -sV -sC -T4 <ip>`
- run one or more specified scripts `nmap -sS -sV -p- --script=< script> `
- nmap scan for udp: `nmap -sU -T4  <ip>`
- check if firewall "filtered" `-sA`
-
- SYN stealt flag scan `-sS`
- TCP Scan `-sT`
- TCP SYN ping : `nmap -sn -PS 10.10.x.x`
- TCP SYN ping with ports : `nmap -sn -PS1-1000 10.10.x.x`
  Quicker, but only shows if hosts are up or not
- First `nmap -sn -v -T4 10.10.x.x`
- second with port findings `nmap -sn -PS21,22,80,445`
- UDP Scan `nmap -Pn -sU <ip>`

- Zone transfer with: `dnsenum google.com`: https://ankisinha.medium.com/dnsenum-a-command-line-information-gathering-tool-a535078207a6 , `$ fierce --domain google.com --subdomains accounts admin ads` : https://github.com/mschwager/fierce

### SMB

- list supported protocols and dialects of an SMB server: `nmap -p445 --script smb-protocols <ip>`
- get information about security level: `nmap -p445 --script smb-security-mode <ip>`
- enumerate users: `nmap -p445 --script smb-enum.sessions <ip>`
- enumerate users through an SMB share using credentials: `nmap -p445 --script smb-enum-sessions --script-args smbusername=<username>,smbpassword=<password>  <ip>`
- enumerate all available shares: `nmap -p445 --script smb-enum-shares <ip>`
- enumerate shares with credentials: `nmap -p445 --script smb-enum-shares --script-args smbusername=<username>,smbpassword=<password>  <ip>`
- enumerate the windows users: `nmap -p445 --script smb-enum-users --script-args smbusername=<username>,smbpassword=<password>  <ip>`
- information about server statistics: `nmap -p445 --script smb-server-stats --script-args smbusername=<username>,smbpassword=<password>  <ip>`
- enumerate available domains: `nmap -p445 --script smb-enum-domains --script-args smbusername=<username>,smbpassword=<password>  <ip>`
- enumerate available user groups:`nmap -p445 --script smb-enum-groups --script-args smbusername=<username>,smbpassword=<password>  <ip>`
- enumerate services: `nmap -p445 --script smb-enum-services --script-args smbusername=<username>,smbpassword=<password>  <ip>`
- enumerating all the shared folders and drives then running the ls command in each shares: `nmap -p445 --script smb-enum-shares,smb-ls --script-args smbusername=<username>,smbpassword=<password>  <ip>`
