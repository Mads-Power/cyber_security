# Footprinting and scanning notes

## Network protocols

![alt text](/assets/OSImodel.png)

- bits = zeros and ones
- OSI = Open Systems Interconnection

## Network layer

- layer 3
- responsible for addressing, routing and forwarding data packets between devices and across different networks
- primary goal is to determine optimal path for data travel
  Protocols: Internet prtocol(IP), Internet Control Message Protocol(ICMP)

IP Header Format

- ![alt text](/assets/IP_header_format.png)

## Transport layer 4

- end to end communication, handling tasks , error detection
- protocols: TCP, UDP
  Well known ports and regisered ports/services:
  - 80: HTTP - 443: HTTPS
  - 21: FTP
  - 22: SSH
  - 25: SMTP(Simple mail transfer protocol)
  - 110: POP3(Post Office Protocol version 3)
  - 3389: Remote Desktop Protocol
  - 3306: MySQL Database
  - 8080: HTTP alternate port
  - 27017: MongoDB database

![alt text](/assets/tcp_udp_comparison.png)

## Network Mapping / Host discovery

### ping sweeps

- network scanning techniques, discover live hosts.

  ICMP Echo Request

  - Type: 8
  - Code: 0
    ICMP Echo Reply:
  - Type: 0
  - Code: 0

    = Host is online

  ### Host Discovery with Nmap

  - `nmap -sn <10.10.x.x/24> --send-ip` <-- scanning subnet hosts
  - start wiresharp whn scanning with nmap to get the full picture(maybe)
  - scanning different targets `nmap -sn 10.10.x.200-224`
  - - TCP SYN ping : `nmap -sn -PS 10.10.x.x`
  - TCP SYN ping with ports : `nmap -sn -PS1-1000 10.10.x.x`

  ### Service version and OS Detection

  - service version `-sV`
  - OS detection `-O`
  - focus aggresive scan on OS detection kernel`-O --osscan-guess`
  - aggresive service detectiom `-sV --version-intensity <number f.eks:8(high)>`

### Nmap scripting engine(NSE)

- look at different scripts: `ls -al /usr/share/nmap/scripts/ | grep -e "http"`
- `http-enum.nse`
- Checks for the HTTP response headers `http-security-headers.nse`
- default script scan `-sC`
- more info on script `nmap --script-help=<script name>`
- run one or more specified scripts `nmap -sS -sV -p- --script=< script> `
- if FTP found, usfull script to run: `--script=ftp-anon`
