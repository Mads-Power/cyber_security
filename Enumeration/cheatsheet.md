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
