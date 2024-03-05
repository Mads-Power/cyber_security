#  Network

### The OSI model
    - layer 7 = applcation
    - layer 6 = presentation
    - layer 5 = session
    - layer 4 = transportation
    - layer 3 = network
    - layer 2 = data link 
    - layer 1 = physical
  
    Layer 7 application: works with applications to provide a interface to transmit data.

    Layer 6 presentation: standardises the format recived from the the application level and handles the encryption, compression and other transformations to the data.

    Layer 5 Session: will check if it can set up a connection to the other computer on the network, if it cant't it will return an error, if it can then it is then session layers job to maintain the connection and to synchronise communication with the other computer. the session it creates is unique and is what allows you to make multiple requests to different enpoints at the same time without the data gettiong mixed up(to tabs in the brower example).

    Layer 4 Transport: first purpose is to choose protocols(TCP, UDP ). TCP is connection-based and will maintain connection for the duration of the request. allows for reliable transmission. all packets get to the right place.
    UDP the opposite, think of laggy video tranmission. 
    When protocol is chosen it devides the transmission up into bite-sized packets(TCP = segments, UDP = datagrams).

    Layer 3 Network: Has the job to locate the destination of your request, so it uses IP addresses(Logical sddressings) to locate the webpage or destination and figures out the best route to take. IP addresses is designed to create order to networks by categorising and sorting them. commin logial addressing(ip addresses) IPV4 IPV6.

    Layer 2 Data Link: Focuses on physical addressing, takes the IP address given to it by the network layer then ads in the physical MAC address of the reciving endpoint. Inside every network enabled computer is a NIC(Network Interface Card) wich comes with a unique MAC(Media Access Control) address to identify it.  Mac addresses can't be changed but they can be spoofed. 
    Data link layer has also the job to present the data in a format suitable for transmission.

    Layer 1 Physical: ardware of the computer.
