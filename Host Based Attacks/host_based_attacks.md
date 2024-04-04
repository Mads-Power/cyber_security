# Host Based Attacks

- Targeted against a specific host/system

## Overview over Windows vulnerabilities

**Types of windows vulnerabilities (not complete)**

- Information Disclosure
- buffer overflows
- Remote code execution
- privilege escalation
- Denial of Service

**Frequently Exploited Windows services**

- **|** Microsoft IIS **|** TCP Ports 80/443 **|** Proprietary web server **|**
- **|** WebDav(Web Distributed Authoring & versioning) **|** TCP Ports 80/443 **|** HTTP extention that Allows client to update, delete , move and copy files on a web server. WebDAV is used to enable a web server to act as a file server. **|**
- **|** SMB/CIFS(Server Message Block Protocol) **|** TCP port 445 **|** Network file share on a LAN **|**
- **|** RDP **|** TCP port 3389 **|** Remotely authenticate and interact with a Windows system **|**
- WinRM(Windows Remote Management Protocol) **|** TCP ports 5986/443 **|** Windows remote management protocol that can be used to facilitate remote access with windows systems. **|**

**Exploiting Microsoft IIS WebDAV**
