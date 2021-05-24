

[toc]

# Unit 1 Internet and IP

The Internet dominant model is **a bidirectional, reliable byte stream.**
Three networked applications:

- World Wide Web(HTTP): client-server model
- BitTorrent: peer-to-peer model, *client - Tracker - other clients*
- Skype: Mix of the both, *depends on if Skype clients can directly commu*

## The 4 Layer Internet Model

Data be sent by unit packets.

The network-layered **packet** be drawn as datagram, containing **data, source, and destination.** Source and destination info part is called **header.**

<img src="img/image-20210523023224721.png" alt="image-20210523023224721" style="zoom:50%;" />

We use the Internet Protocol(IP) as a must:

- IP makes a best effort attempt to deliver datagrams, but no promises.
- IP datagrams can be lost, delivered out of order, corrupted, noguarantees.
- To deal with the unpromised issuedm the other protocol on the top of IP needed in Transport Layer (i.e., Transmission Control Protocol).

TCP:

- In Transport Layer
- Makes sure that all data packets delivered **correctly, in order,  to right place.**

UDP (User Datagram Protocol):

- Also in Transport Layer
- Does **not guarantee** the correct deliver, no repetition, in order. But **higher efficiency**.

<img src="img/image-20210523023406505.png" alt="image-20210523023406505" style="zoom:45%;" />

IP is called “thin waist”, for there are many choices on the top and under the Network Layer, but in Network Layer IP is the only choice.

| Application Layer:                                           | Transport Layer:                                             | Network Layer:                                               |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| HTTP: <br/>- Generate request message according to target web server.<br/>- Precoss the requested content. | TCP:<br/>- Devided the HTTP message into segments, in sequence. Then pass them reliably.<br/>- Receive the segments and re-structure the seg,ents in sequence. | IP:<br/>- Searching for target address, passing while routing. |

<img src="img/image-20210523023428485.png" alt="image-20210523023428485" style="zoom:43%;" />

## The IP Service

<img src="img/image-20210523023441170.png" alt="image-20210523023441170" style="zoom:47%;" />

**The IP service model**:

In which the packet jumps among the routers (in which there are routing tables) in the path towards its destination. Routers in the path dunno its destination but the each other of themselves.

<img src="img/image-20210523023452709.png" alt="image-20210523023452709" style="zoom:45%;" />

**The reasons for IP being so simple:**

- Simple, dumb, minimal = faster, more streamlined and lower cost to build and maintain.
- The end-to-end princeple: Implement features in the end hosts as much as possible.
- Allows a variety of dependable/unreliable services to be built on top.
- Works over any link layer: IP makes few assumptions about the link layer below.

**More features of IP service model:**

1. Tries to prevent packets looping forever.
2. Will fragment packets if they are too long.
3. Use a header **checksum** to reduce chances of delivering datagram to wrong destination.
4. Allow for new versions of IP
   - IPv4 with 32-bit address
   - IPv6 with 128-bit address
5. Allow for new options to be added to header.

### What’s in a Datagram

<img src="img/image-20210523023530773.png" alt="image-20210523023530773" style="zoom: 43%;" />

- The most important parts are **DA (Destination IP address)** and **SA (Source IP Address)**.
- Protocol ID part tells what is in data field, helping destination host correctly process the packet.
- Version field tells the version of the IP.
- TTL part helps to stop the packet looping in teh Internet. Every router will decrement the TTL till it reaches 0.
- The packet ID, Flags, Fragment Offset help router to fragment IP packet into smaller self-contained packets if needed.
- Type of Service tells the importance of the packet.
- Header Length tells how big the header is.
- Checksum is calculated over the whole header to make sure the packet to correct destination.

*Application Layer - stream of data*

*Transport Layer - segments of data*

*Network Layer - packet of data*

In summary, IP provide a deliberately simple services:

- Datagram
- Unreliable
- Best-effort
- Connectionless

## Life of a Packet

### TCP byte stream

**Three-way handshake: *“SYN, SYN/ACK, ACK”***

*form Client to Server:* Send a synchronized message, ***“SYN”***

*from Server to Client:* Response a synchronized message and acknowledges the clent’s synchronized, ***“SYN/ACK”***

*from Client to Server:* Acknowlwdge server’s synchronize, ***“ACK”***

### Inside the Stream

*Address format:* **IPAddress:TCPPort** (e.g., **192.168.0.1:22**)

Above is how a normal address looks like, IP address in Network Layer and TCP port in Transport Layer.

**A forwarding table in a router:**

<img src="img/image-20210523023511449.png" alt="image-20210523023511449" style="zoom:50%;" />

The router will send the packet to the matched entry pattern, which is the ***most specific*** match.

The default route is the least specific route, matches every IP address.

### The tools

**Wireshark:** Show all the packets, TCP byte stream establishment and data exchange

**Traceroute:** Shows the route (all the routers it passed) that the packet took through Internet

## Principle: Packet Switching

**Packet:** A self-contained <u>unit of data</u> that carries necessary <u>info</u>rmation to reach <u>destination</u>.

<img src="img/image-20210523140136574.png" alt="image-20210523140136574" style="zoom: 43%;" />

The packet will be routed between routers until it reached destination.

**Packet switching:** Independently for each arriving packet, pick its outgoing link. Send it if the link is free, else hold it for later.

Source routing/self-routing: The source sepecifies the route. (Not currently common for its big security issues)

Nowdays we use the route table that in each switch, according to which the switch can tell the packet to destination goes to right next hop.

Two consequences:

1. Simple packet forwarding
2. Efficient sharing of links

### No per-flow state required: Simple packet forwarding

**Flow:** A collection of datagrams belonging to the same end-to-end communication, such as a TCP connection.

Packet switching don’t need state for each flow, for each packet is self-conatined.

- No per-flow state to be added/removed
- No per-slow state to be stored
- No per-flow state to be changed upon failure

In one word, packet switching cares only about switching, do not deal with other issues!

### Efficient sharing of links

*\*bursty: Occrurring at intervals in short, sudden episodes\**

Data traffic is bursty

- Packet switching allows flows to use all available link capacity.
- Packet switching aloows flows to share link capacity.