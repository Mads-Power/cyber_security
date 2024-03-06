#  Network

### The OSI model
    - layer 7 = applcation
    - layer 6 = presentation
    - layer 5 = session
    - layer 4 = transportation
    - layer 3 = network
    - layer 2 = data link 
    - layer 1 = physical
  
    Layer 7 application: works with applications to provide a interface to transmit data. Accepsts  or rejects requests from applications.

    Layer 6 presentation: standardises the format recived from the the application level and handles the encryption, compression and other transformations to the data.

    Layer 5 Session: will check if it can set up a connection to the other computer on the network, if it cant't it will return an error, if it can then it is then session layers job to maintain the connection and to synchronise communication with the other computer. the session it creates is unique and is what allows you to make multiple requests to different enpoints at the same time without the data gettiong mixed up(to tabs in the brower example).

    Layer 4 Transport: first purpose is to choose protocols(TCP, UDP ). TCP is connection-based and will maintain connection for the duration of the request. allows for reliable transmission. all packets get to the right place.
    UDP the opposite, think of laggy video tranmission. 
    When protocol is chosen it devides the transmission up into bite-sized packets(TCP = segments, UDP = datagrams).

    Layer 3 Network: Has the job to locate the destination of your request, so it uses IP addresses(Logical sddressings) to locate the webpage or destination and figures out the best route to take. IP addresses is designed to create order to networks by categorising and sorting them. common logial addressing(ip addresses) IPV4 IPV6.

    Layer 2 Data Link: Focuses on physical addressing, takes the IP address given to it by the network layer then ads in the physical MAC address of the reciving endpoint. Inside every network enabled computer is a NIC(Network Interface Card) wich comes with a unique MAC(Media Access Control) address to identify it.  Mac addresses can't be changed but they can be spoofed. 
    Data link layer has also the job to present the data in a format suitable for transmission.

    Layer 1 Physical: Hardware of the computer.

## TCP / IP model
    Encapsulation and de-encapsulation

    - Application
    - Transport
    - Internet
    - Network Interface(newer versons split tis up into Data Link and physical layers)
    
    Matching up the OSI model with the TCP/IP model:
        - OSI: Application + Presentation + Session = TCP/IP: Application
        - OSI: Transport = TCP/IP: Transport
        - OSI: Network = TCP/IP: Internet
        - OSI: DataLink + Physical = TCP/IP: Network Interface

    Three-way handshake
        - SYN(synchronise)-SYN/ACK-ACK
        client to server -> SYN ? 
        Server to client -> SYN/ACK
        client to server -> ACK


## Subnetting 
    subnetting splits up the number of hosts that can fit within the network, reprsented by a subnet mask. a subnet is representet as a number of four bytes(32 bits) ranging from 0 to 255.

## IP address explanation
    an IP address has four sets or octets of numbers each with the value up to 255. These are known as IP classes. Each is based on the first octet of IP addresses and is classified as AB or C. it the first octet begins with 0 bit, then it is of type class A. 
    Class A has a range up to 127.x.x.x If it starts with bits 10, it belongs to class B and ranges from 128.x to 191.x. Finally, the IP class belongs to class C if the octet starts with bits 110 and has a range rom 192.x to 223.x. 

    Classes from A,B and C can be used for Host Addresses
    Starting Address = SA
    Ending Address = EA
    Class A = SA: 0.0.0.0 , EA: 127.255.255.255
    Class B = SA: 128.0.0.0 , EA: 191.255.255.255
    Class C = SA: 192.0.0.0 , EA: 223.255.255.255

    Class D is for multicast and Class C for multipurposes
    ------------------------------------------------

    