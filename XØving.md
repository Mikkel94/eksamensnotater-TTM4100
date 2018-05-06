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

Important abbreviations:
|Abbreviation|Explanation|
|--|--|
|ECE|Explicit Congestion Notification Echo (routers notify recv that notifies sender)|
|ECN|Explicit Congestion Notification|
|MSS|Maximum Segment Size|