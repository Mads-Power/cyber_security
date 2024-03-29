# HOST to HOST

## Network characteristics

- topology
- Speed
- cost
- Security
- Availability
- Scalability
- Reliability

## The OSI Reference Model owerview

Upper layers:

---

- 7 application
- 6 presentation
- 5 session

Lower Layers = network engineers

---

- 4 Transport = TCP/UDS,port
- 3 Network = IP, Devices: Routers
- 2 Data-Link = Ethernet MAC Address, Devices: switches
- 1 Physical

## OSI Acronyms

The classic: Please Do Not Throw Sausage Pizza Away

## TCP/IP stack

- Application: Data
- Transport: Segment
- Internet: Packet
- Network Access: Frame

### The lower OSI LAyers

- 4 Transport layer =

  - if TCP or UDP is being used and the port number.
  - The transport Layer defines services to segment, transfer, and reassemble the data for infidividual communications between devices.
  - It Breaks Down large files into smaller segments that are less likely to incur transmission problems.

- layer 3 Network =

  - the most important information at the network layer is the source and destination IP address
  - router operate at layer 3
  - The network layer provides connectivity and path selection between two host systems that may be located on geographically separated networks.
  - the network layer is the layer that manages the connectivity of hosts by providing logical addressing.

- layer 2 The Data-Link Layer
  - The most important information at the data-link layer is the source and destination layer 2 address
  - for example the source and destination MAC address if Ethernet is the layer 2 technology
  - switches operates on layer 2
  - the datalink layer defines how data is formatted for transmission and how access to physical media is controlled.
  - it also typically includes error detection and correction to ensure a reliable delivery of the data.
- Layer 1 Physical layer

  - concerns physical components of the network
  - the physical link enables bit transmission between end devices
  - it defines specifications needed for activating , maintaining and deactivating the physical link between end devices.

  ### The CISCO IOS operating systems

  Navigating CISCO command line:
  Router:

  - To see commads at user exec mode type `?`
  - To go to privilege exec mode type `enable`
  - To see comands "starting with" example `di?`, use start letters the question mark, use space to get more context with that command `dir ?`
  - `show` command used lot
  - `debug`
  - global terminal `configure terminal`
  - `ctrl+a` move to start of line
  - if you are at the wrong command level you will also get a _"invalid input at '^' marker"_ , if you get it you can write `do` first
  - command commands: `do show ip interface brief`, ` do show running-config`

  ## OSI Layer 4 - Transport layer

  - the transport layer provides transparent transfer of data between hosts and provides end-to end error recovery and flow control.
  - flow control is the process of adjusting the flow of data from the sender to ensure the reciving host can handle all of it.
  - session multiplexing , multiple sessions
  - TCP: connection oriented,
