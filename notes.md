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

<img src="/home/han/.config/Typora/typora-user-images/image-20210523011317634.png" alt="image-20210523011317634" style="zoom:67%;" />

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

<img src="/home/han/.config/Typora/typora-user-images/image-20210523012018158.png" alt="image-20210523012018158" style="zoom:60%;" />

IP is called “thin waist”, for there are many choices on the top and under the Network Layer, but in Network Layer IP is the only choice.

| Application Layer:                                           | Transport Layer:                                             | Network Layer:                                               |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| HTTP: <br/>- Generate request message according to target web server.<br/>- Precoss the requested content. | TCP:<br/>- Devided the HTTP message into segments, in sequence. Then pass them reliably.<br/>- Receive the segments and re-structure the seg,ents in sequence. | IP:<br/>- Searching for target address, passing while routing. |

<img src="/home/han/.config/Typora/typora-user-images/image-20210523012810628.png" alt="image-20210523012810628" style="zoom:50%;" />

## The IP Service

<img src="/home/han/.config/Typora/typora-user-images/image-20210523013017714.png" alt="image-20210523013017714" style="zoom:50%;" />

**The IP service model**:

In which the packet jumps among the routers (in which there are routing tables) in the path towards its destination. Routers in the path dunno its destination but the each other of themselves.<img src="/home/han/.config/Typora/typora-user-images/image-20210523013234608.png" alt="image-20210523013234608" style="zoom:57%;" />

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

<img src="/home/han/.config/Typora/typora-user-images/image-20210523014241591.png" alt="image-20210523014241591" style="zoom:40%;" />

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

*form Client to Server:* Send a synchronized message, “SYN”

*from Server to Client:* Response a synchronized message and acknowledges the clent’s synchronized, “SYN/ACK”

*from Client to Server:* Acknowlwdge server’s synchronize, “ACK”

### Inside the Stream

*Address format:* IPAddress:TCPPort

Above is how a normal address looks like, IP address in Network Layer and TCP port in Transport Layer.

**A forwarding table in a router:**

<img src="/home/han/.config/Typora/typora-user-images/image-20210523020159039.png" alt="image-20210523020159039" style="zoom:50%;" />

The router will send the packet to the matched entry pattern, which is the ***most specific*** match.

### The tools

## Principle: Packet Switching

### No per-flow state required: Simple packet forwarding

### Efficient sharing of links