# Enumeration notes

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

### SMBMap

- null password `smbmap -u guest -p "" -d . -H 10.10.x.x`
- with usernam and password: `smbmap -u username -p password -d . -H 10.10.x.x`
- run command(oåconfig): `smbmap -H 10.10.x.x -u username -p password -x 'ipconfig'`
- show content of share: `smbmap -H 10.10.x.x -u username -p 'password' -r 'C$'`
- upload file from attacker to victim: `smbmap -H 10.10.x.x -u username -p 'password' --upload 'path/file/to/upload' 'smbshare\file'`
- download a file: `smbmap -H 10.10.x.x -u username -p 'password' --download 'smbshare\file'`

### Samba

If you have found samba

- null session rpcclient `rpcclient -U "" -N 10.10.10.x` - When connencted, server info `srvinfo`
- enum4linux get OS info: `enum4linux -o 10.10.x.x `
