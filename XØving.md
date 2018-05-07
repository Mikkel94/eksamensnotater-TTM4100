### Application Layer

**Two predomenant architectures,**
* P2P
* client-server architecture

**Client-server architecture:**
* Always-on host (server)
* Services requests sendt by clients
* Clients don't directly communicate
* Fixed IP address
* Data centers with multiple servers used to handle big payloads by creating a big virtual server with all of them together

**Peer-to-peer (P2P)**
* Traffic client to client
* Good for traffic intensive applications
* BitTorrent, skype etc. is based on P2P
* self scalable
* cost effective
* problems with:
    * security
    * performance
    * reliability (because of highly decentralized structure)

Processes communicate by exchanging messages over a network

**Client and server process**
Process that is initiating communication is labeled as client, process that waits to be contacted is labeled server

**Interface between the process and the computer network**
* A process sends and receives messages through a software interface called a socket
* Host is identified by IP address, process is identified by port number
    * Port 25 = SMTP
    * Port 80 = web server
    * Port 53 = UDP

**Transport services available to applications**
* Can broadly classify the possible services along four dimensions:
    * Reliable data transfer
    * Throughput
    * Timing
    * Security

**TCP services**
* Includes a:
    * **Connection-oriented service**
        * Exchange transport layer control information before the application-level messaging begins (handshake)
        * After the handshake a TCP connection is said to exist between the sockets
        * Full-duplex connection (both processes can send messages to each other at the same time)
    * **Reliable data transfer service**
        * Can count on TCP to deliver all bytes in the same order without duplicates or missing bytes
    * **Congestion control mechanism**
        * TCP throttles a sending process when the network is congested between sender and receiver
* Can be improved by SSL (Secure Socket Layer) (or TSL - Transport Layer Security)
    *

**UDP services**
* Lightweight transport protocol
* Unreliable data transfer service
* no congestion control (can pump data into the network layer at any rate it pleases)

**Application layer protocols**
* SMTP - Simple Mail Transfer Protocol
* Telnet - Teletype Network
* HTTP - Hypertext transfer Protocol
* FTP - File Transfer Protocol
* SIP - Session initiation protocol
* RTP - Real-time Transfer Protocol

**HTTP**
* [RFC 1945] & [RFC 2616]
* Implemented on two programs: Client program & Server program
* HTTP is a stateless protocol (server retains no information about clients)

**HTTP status codes**
* 200 OK - Request succeeded and the information is returned in the response
* 301 Moved Permanently - Request object has been permanently moved; the new URL is specified in *Location:* header of the response message. The client software will automatically retrieve the new URL (if your not using a shitty hack you made yourself in a summer)
* 400 Bad Request - Generic error code indicating that the server did not understand the request
* 404 Not Found - Requested document does not exist on the server
* 505 HTTP Version Not Supported - Requested HTTP protocol version is not supported by the server
* 304 Not Modified - HTTP is not modified since Last-Modified: \<date> (used with conditional GET)

**POP3 Post Office Protocol**
* Client opens TCP connection to the mail server (PORT 110)
* Authorization - authenticates the user with username and password
* Transaction - user agent retrieves messages (can also mark for deletion, remove deletion marks, and obtain mail statistics)
* Update - occurs after quit command, ends the POP3 session, deletes all messages marked for deletion

**DNS Message**
* First 12 bytes = Header section (has a number of fields)
    * First Field = 16bit number that identifies the query (copied into reply message to a query, allowing a client to mtach received replies with sent queries)
    * Then there is a number of flags in the flag field
* Question section (for queries)
* Answer section (for replies)
* Authority section (contains records of other authorative servers)
* Additional section (contains other helpful records)
* DNS records are stored in 4-tuples (Name, Value, Type, TTL)

Can use *nslookup* program to sebd DBS query message

**Content Delivery Network (CDN)**
* **Enter Deep**
    * Deploy servers in access ISPs all over the world
    * Gets close to end users
    * Improves user-perceived delay
    * Decreasing number of links and rouser between end user and CDN server
    * Maintaining and managing clusters become difficult (because of the highly distributed design)
* **Bring Home**
    * Build large server clusters in fewer locations than Enter Deep
    * CDNs are placed in IXPs (Internet Exchange Points) not ISPs
    * Lower maintenance and management overhead
    * Higher delay end-to-end and lower throughput to users
* Storage of content works very similar to web caching (pull caching) in some cases. Netflix for example does not use pull caching. Netflix instead pushes out content in "off-hours"
* We use DNS to route to correct CDN so that users can get optimal performance
* **Cluster selection strategies**
    * Geo-location (routed closest CDN)
    * Periodic real-time measurements of delay and loss


### Transport Layer

**Go-Back-N (GBN)**
* A GBN sender must respond to three types of events:
    * Invocation from above
        * The sender needs to check if window is full, and send if it isn't or buffer/send back/not accept attempts to send (with f.ex. a semaphore or a flag) if the window is full
    * Receipt of an ACK
        * Cumulative ACKs
    * A timeout event
        * Sender resends all un-ACKed packets
* With large windows it can be slow and do many unnecessary repeats, because it retransmits everything in the window sendt after a lost packet because GBN does not buffer

**Selective Repeat (SR)**
* Retransmits only the packets that it suspects are lost
* Window doesnt move before all ACKs in window is received
* Buffers out-of-order packets
* Each packet has it's own logical timer (mimiced by a single hardware timer)
* Window size must be $\leq$ to the sequence number space for SR protocols

COMMENT: The assumed lifetime cycle of a packet in a high speed network is 3 minutes [RFC 1323] (this to avoid multiple packets with the same sequence number)

**TCP**
* Contains following header fields:
    * Source and destination port numbers
    * Checksum field
    * 32-bit sequence number field / 32-bit acknowledgement number field (for reliable data transfer)
    * 16-bit receive window (used for flow control)
    * 4-bit header length field (specifies length of the TCP header in 32-bit words)
    * 6-bit Flag field. The ACK bit is used to indicate that the value carried in the acknowledge field is valid. The RST, SYN and FIN bits are used for connection setup and teardown. The CWR and ECE bits are used in explicit cingestion notification. Setting the PSH bit indicates that the receiver should pass the data to the upper layer asap. finally the URG bit is used to indicate thata there us data in this segment that the sending-side upper-layer entity has market as "urgent" (PSH, URG is not used in practice)
    * Options field (optional) (used when a sender and receiver negotiate maximum segment size (MSS) or as a window scaling factor for use in high-speed networks)
* Cumulative acknowledgements
* Can both discard and keep (in buffer) out-of-order segments. Using a buffer is what is used in practice since it causes less strain on the network
* Can open multiple TCP connections, so it is not necessarily fair even though it uses congestion controll (this will make other connections from other PCs use less bandwidth)

### Network Layer: Data Plane

**What's inside a router?**
* Input ports
    * Performs a lookup function to decide which output port to forward to
    * Forwards control packets to the routing processor
* Switching fabric
    * Connects input ports to output ports
* Output ports
    * Stores packets received by switching fabric and transmits these packets on the outgoing link
* Routing processor
    * Performs control plane functions
    * Executes routing protocols
    * Maintains tables and attached link state information
    * Computes the forwarding table for the router

**Types of forwarding**
* Destination based forwarding
    * Forwards depending on final destination
    * Quickest way for that specific datagram
* Generalized forwarding
    * Forwards depending on a number of factors
        * such as:
        * Origin
        * Type of datagram
        * and whatever else you may please

**Forwarding**
* Longest prefix matching
* Must be superfast. F.ex. be able to handle 64byte IP datagrams on a 10 Gbps link (in this case with just one input, the processing will have to take only 51.2 nanoseconds)

**Switching**
* Switching via memory
    * Switching is done under direct controll of CPU and input/output functions as standard I/O devices
    * If memory bandwidth is such that a maximum $B$ packets per second can be written into, then overall forwarding throughput must be $\leq B/2$
    * Two packets cannot be forwarded at the same time (because of shared bus)
    * Modern memory switches look a lot like shared-memory multiprocessors
* Switching via bus
    * Switches directly over the bus
    * Only one packet can cross the bus at a time
    * Marks each packet and sends it to every output. Outputs that don't "want" it discards it
* Switching via interconnected network
    * Multiple buses
    * Can forward in parallell to different output ports
    * Can still not send multiple packets to the same output port at the same time
    * Can use multistage switching fabric to be able to achieve concurrency
    * Or multiple switching fabrics to achieve parallallelity

**Packet Scheduling**
* First-In-First-Out
    * Selects packets for link transmitting in the same order as they arrived at the output link queue
* Priority Queuing
    * Arriving packets at teh output link are classified by classes such as:
        * Network management
        * SMTP
        * IMAP
        * HTTP
        * etc.
    * Depending on what class the packets are they will be given a different priority level
    * Usually FIFO scheduling is done within each class
* Round Robin and Weighted Fair Queuing (WFQ)
    * Round Robin:
        * Sorts packets into classes
        * Then picks out one from each class each "turn". Like picking out a team for gym in 3rd grade
    * WFQ:
        * Sorts packets into classes
        * Serves classes in a circular "motion" as with RR
        * Differs from RR that each class may receive a different amount of service in any interval of time

**IPv4 Datagram format**
Key Fields:
* Version number (4 bits)
    * Which version of IP (f.ex. IPv4 or IPv6)
    * Router uses it to determine how to interpret the rest of the IP datagram
* Header length (4 bits)
    * Used to determine where the payload begins
    * most IP datagrams don't contain options, so the usual header length is 20 bytes
* Type of service
    * Used to determine what kind of IP datagram it is. F.ex. FT, RTP etc.
* Datagram length (16 bits)
    * Total length of the IP datagram (header + payload)
* Identifier, flags, fragmentation offset
    * Have to do with IP fragmentation
    * IPv6 does not allow fragmentation
* Time-to-live
    * Used so that a datagram will not circulate forever
    * Field is decremented by one each time the datagram is processed by a router
    * When it is 0, the datagram must be dropped by the router
* Protocol
    * The value of this field indicates which transport layer protocol teh payload in this datagram should be passed to.
* Header checksum
    * Aids routers in detecting errors in the IP datagram headers
* Source and destination IP addresses
* Options
    * Exist so that we are able to extend the IPv4 header
    * Makes things alot more difficult and time consuming
    * No options in the IPv6 headers
* Data (payload)
    * Contains the data from the transport layer

**IPv4 Datagram fragmentation**
* When Maximum Transmission Unit (MTU) is smaller than the length of the IP datagram
* Destination host reassembles the fragmented pieces
* Fragmentation offset, identification and flag fields are used by destination host to figure out how to reassemble the fragmented pieces, and to figure out if it got all of the pieces correctly.

**IPv4 Addressing**
* Defining the subnets in the system: *To determine the subnets, detatch each interface from its host or router, creating islands of isolated networks, with interfaces terminating the end points of the isolated networks. Each of these isolated network is called a **subnet***
* Internet's address assignment strategy: **Classless Interdomain Routing (CIDR)**
    * 32 bit IP address is divided into two parts:
        * Format: a.b.c.d/x where x indicates the number of bits in the first part of the address
* OLD: Classful addressing
    * inefficient (alot of unused address space)
    * could only have 8, 16 and 24-bit subnets
* Host addresses are typically configured using the **Dynamic Host Configuration Protocol(DHCP)**
    * zeroconf (also called plug-and-play protocol
    * dynamically allocates IP addresses as needed (very useful for when laptops are moved around in different subnets, like on a university campus)
    * Four step protocol:
        * DHCP server discovery
            * Uses UDP to send a DHCP discover message to (255.255.255.255, 67) which is to all nodes on the subnet on port 67 which is UDP port
        * DHCP server offer(s)
            * DHCP server sends back (on broadcast 255.255.255.255) offer with IP address, network mask and a lease time
        * DHCP request
            * Client sends back request for one of the offers that it has gotten
        * DHCP ACK
            * Server responds with ACK
    * Cannot maintain TCP connection as it moves (because IP address changes when computer changes subnet)
        * Mobile IP fixes this

**Network Address Translation (NAT)**
* A NAT router looks like a single device with one unique IP to the outside world
* Uses DHCP to allocate private IP addresses within the home network
* NAT translation table translates between port numbers and private IP addresses so that requests/responses may be routed correctly within the home

**IPv6**
* Created because IPv4 was beginning to run out of address space
* IPv6 Datagram Format:
    * Increased address space (128 bits)
    * Anycast address (allows a datagram to be delivered to any one of a group of hosts). Can be used to send a HTTP GET request to a number of mirror sites that contain a given document
    * Streamlined 40-byte header
        * Fixed length allows for faster processing of the IP datagram by the router
        * New encoding of options allows for more flexible options processing
    * Flow labeling (to label f.ex. RTP or users paying more) so they get priority in routers
* IPv6 Header Fields:
    * Version
        * Same as IPv4
    * Traffic class
        * Same as TOS in IPv4
    * Flow label
        * 20 bit field
        * used to identify a flow of datagrams
    * Payload length
        * Length of the payload (data following the 40 byte header)
    * Next header
        * Identifies the protocol to which the payload of the datagram, should be delivered (TCP, UDP, etc.)
        * Uses same values as protocol header field in IPv4
    * Hop limit
        * Content are decremented each time the datagram is processed by a router
        * Datagram is discarded once the Hop limit eaches zero
    * Source and destination address
        * Self explanatory
    * Data
        * The payload
        * Will be removed from the datagram and passed on to the protocol defined in the Next header field
* Removed fields (when going from IPv4 to IPv6):
    * Fragmentation/reassembly
        * Took a long time
        * Instead routers send a "packet too big" message
        * Resizing of packets only done by hosts
    * Header checksum
        * Took a long time to recalculate at each router
        * Transport layer already covers this, so not really needed to do in the network layer
    * Options
        * No longer a field within the IP datagram, but can be included as a header in Next header, which can point to options
        * This was removed to have a fixed 40 byte header, making things easier and faster

**Transition from IPv4 to IPv6**
* Is super slow (the transition)
* Now we do alot of it via tunneling
* Tunneling is packing a IPv6 datagram in the payload of a IPv4 datagram when you have to go from a IPv6 router to another IPv6 router but there is IPv4 routers inbetween you have to go through

### Network Layer: Control Plane

In this chapter: How are forwarding and flow tables computed, maintained and installed
* Two possible approaches
    * Per-router control
        * A routing algorithm runs in every router
        * Each router has a routing component that communicates with routing components in other routers to compute values for forwarding tables
    * Logically centralized control
        * A logically centralized routing controller computes data from lots of routers and sends back routing tables. This abstraction allows the router to perform traditional IP forwarding, aswell as load sharing, firewalling, and NAT. This otherwise has to be done in sepparate middleboxes
        * Seems to be good for large scale routing

**Routing algorithms**
* Determines "good" paths from sender to receiver.
* Weird rules (company x cannot be routed through company y routers and petty shit like that)
* An edge is a pair of nodes
* An edge has a value representing its cost (typically the edge's cost reflects the length between the edge's nodes)
* A **centralized routing algorithm** computes the least-cost path between a source and a destination using complete, global knowledge. Algorithms with global state information are often called **Link-State (LS) Algorithms**
* A **decentralized routing algorithm** calculates the least-cost path in an iterative manner. Each router talks to its neighboring routers and together figures out the best path. No node has all of the global state information
* Static routing algorithms change very slowly over time, usually as a result of human intervention
* Dynamic routing algorithms change alot more and faster, but are susceptible to problems such as routing loops and route oscillation
* Load sensitive routing algorithms calculates routes based on congestion
* Load insensitive routing algorithms don't give a damn about congestion when computing cost of a path

**The Internet Control Message Protocol (ICMP)**
* Is used by routers to communicate network-layer information to hosts
* Often used for error messages
    * Cannot reach destination
    * Wrong port
    * TTL expired
    * Packer too big (IPv6 only)
* Ping programs can use it

### Link Layer and LANs

* Two fundamentally different types of Link-Layer channels:
    * Broadcast channels
        * Connect multiple hosts
        * Medium access protocol is needed to coordinate frame transmission. A central controller or the hosts themselves can do this
    * Point-to-point communication link
        * Used over long distances
        * Directly connects two routers
* Most of the Link-Layer is implemented in hardware, some is implemented in CPU software

**Possible services to be offered by a Link-Layer protocol:**
* Framing
    * Encapsulation of datagrams. Contains paylaod (datagrams) and header fields
* Link access
    * A Medium Access Control (MAC) coordinates frame transmissions between nodes (especially in broadcast links where there is multiple nodes to coordinate the rules of transmission for)
* Reliable delivery
    * Mostly just used for links with a possible high error rate such as wireless links. This is achieved with ACKs and retransmissions (just as with TCP)
* Error detection and correction
    * Transmitting node includes error-detection bits in the frame
    * More sophisticated than the internet checksum
    * implemented in hardware

**Error detection and correction techniques**
* Using bit-level error detection and correction
* Checks if an error is detected (not occured, because we don't always catch them)
* Checksum methods is most often used in transport layer, while cyclic redundancy checks are often done i link layer
* **Parity Checks**
    * Sender adds a bit at the end of the sequence they are sending so that the total number of bits value of 1 are even (odd is also possible ofcourse). If the receiver detects an uneven number of bits value of 1, then an error has occured
    * Errors can be detected and corrected with 2-dimensional parity schemes.
    * Can only detect errors in odd pairs (2 errors will make the total of 1s still even)
* **Checksumming Methods**
    * Calculates a checksum from *d* bits of data and uses the 1s complement of this sum to check if errors have occured (if there is any 0s an error has occured)
    * Used in transport layer because it is not very resource intensive and transport layer uses software to calculate
    * Link layer is embedded in hardware so can use more complex error detection and correction techniques without much problem
* **Cyclic redundancy Check (CRC)**
    * Also known as polynomial codes
    * Can detect a burst of errors $<r+1$
    * Can detect all burst of odd number of errors
    * All CRC calculations are done with modulo-2 arithmetic without carries in addition or borrows in subtraction (addition and subtraction are equal and effectively the same as XOR)

**Multiple Access Links and Protocols**
* When many nodes try to broadcast at the same time there will be collision and the receiving nodes won't understand anything -> Bandwidth is wasted
* Three types of multiple access protocols: **Channel partitioning protocols, random access protocols, taking-turns protocols**
* **Channel Partitioning Protocols**
    * Assume you have a channel with $R$ bps
    * Assume you have $N$ nodes
    * **Time-Division Multiplexing (TDM)**
        * Divides $R$ into $N$ time slots within each time-frame, one for each Node
        * Each Node gets $R/N$ bps
        * Everyone gets equal
        * Drawbacks:
            * Everyone must wait for their turn, even if the others don't broadcast anything
            * If the other nodes don't broadcast, time is wasted
    * **Frequency-Division Multiplexing (FDM)**
        * Divides channel into $N$ different frequencies
        * Same drawbacks and pluses as TDM
    * **Code Division Multiple Access (CDMA)**
        * Gives each different node a code to encode its packets with so that the receiver (assuming receiver knows the codes) can decode correct info
        * Multiple nodes can broadcast over the same frequency at the same time without collison
        * Used mainly for wireless
* **Random Access Protocols**
    * Assume you have a channel with $R$ bps
    * Assume you have $N$ nodes
    * Nodes always transmits packets at $R$ bps
    * When there is a collision nodes retransmits packets after a random interval
    * **Slotted ALOHA**
        * Assume the follwing:
            * All frames is $L$ bits
            * $L/R$ is the time it takes to transmit one frame
            * Nodes start to transmit frames only at the beginning of slots
            * Each node knows when the slot begins
            * All nodes detect the collision before the slot ends if two or more nodes collide
        * Calculates a probability if the node is to send in the slot after a collision. Maybe maybe not do so, 37% efficiency
        * All nodes synchronize their transmissions to start at the beginning of a slot
    * **ALOHA**
        * As slotted ALOHA, but without slots, transmits at "random" times, can start transmitting whenever
        * Half as effective as slotted ALOHA
    * **Carrier Sense Multiple Access (CSMA)**
        * Listens to channel, don't send if someone else is transmitting
        * Propagation delay makes collisions happen even though nodes listens on the
    * **Carrier Sense Multiple Access with Collision Detection (CSMA/CD)**
        * Stops sending if collision
        * Waits random amount of time when collides happen (detection of collision)
* **Taking-Turns Protocol**
    * **Polling Protocol**
        * A master node polls each node (round robin)
        * inefficient if not all nodes are active because active nodes have to wait for the polling delay
        * Less than R bps even with only 1 active node
    * **Token-Passing Protocol**
        * Nodes pass a token around (instead of round robin)
* Cable access network - uses multiple access protocols

**Switched Local Area Networks (Switched LANs)**
*