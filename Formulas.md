**Calculating Download speed with N clients.**

$N =$ Number of clients downloading
$F =$ Size of file being downloaded
$u_i =$ Upload speed for a client
$u_s =$ Upload speed for server
$d_i =$ Download speed for a client
$d_{min} = min\{d_1, d_2, d_3, ..., d_N\}$

**Client-Server**
$D_{CS} \geq max\{\frac{FN}{u_s}, \frac{F}{d_{min}} \}$

**Peer-to-Peer + Server**
$D_{P2P} \geq max\{\frac{F}{u_s},\frac{F}{d_{min}},\frac{FN}{u_s + \sum_{i=1}^{N}u_i} \}$

---

**Time needed to transmit a package into a link**

$d_{trans}=$ Time need to transmit into link
$L=$ Packet size
$R=$ Transmission rate

$d_{trans} = \frac{L}{R}$

**Estimated RTT**
$SampleRTT$ = Sampled RTT
$EstimatedRTT$ = Estimated RTT (what we're looking for)
$\alpha = 0.125$ (Recommended in [RFC 6298])

$EstimatedRTT = (1 - \alpha) \cdot EstimatedRTT + \alpha \cdot SampleRTT$

NOTE: $EstimatedRTT$ is a weighted average of the $SampleRTT$ values.

$DevRTT=$ Measurement of how much the $SampleRTT$ typically deviates from the $EstimatedRTT$
$\beta=0.25$ (recommended value)

$DevRTT = (1-\beta)\cdot DevRTT + \beta \cdot | SampleRTT - EstimatedRTT |$

$TimeoutInterval=$ Time before TCP times out and resends segment

$TimeoutInterval = EstimatedRTT + 4 \cdot DevRTT$

**Graph**
$G=$ graph
$N=$ a set of nodes
$E=$ a collection of edges

$G=(N,E)$

**Cyclic Redundancy Check (CRC)**
$D=$ *d* bits of data to be sendt
$G=r+1$ bit pattern (generator) (leftmost bit of $G$ must be a 1)
$R=r$ bits chosen by sender
$d+1$ is exactly divisible by G
If remainder is nonzero, an error has occured

How sender computes $R$:
$R=remainder \frac{D\cdot2^r}{G}$

**CSMA/CD Efficiency**
$d_{prop}=$ maximum time it takes signal energy to propagate between two adapters
$d_{trans}=$ time to transmit a maximum size frame between two adapters

Efficiency $=\frac{1}{1+5d_{prop}/d_{trans}}$

