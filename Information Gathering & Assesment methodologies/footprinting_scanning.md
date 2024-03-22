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
