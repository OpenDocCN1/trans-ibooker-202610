
or User Datagram Protocol (UDP), which are the established standard transport protocols for the IP network stack.

Distributed applications can choose which of these protocols to use. Implementations are widely available in mainstream programming languages such as Java, Python and C++. In reality, use of these APIs is not common as higher-level programming abstractions hide the details from most applications. In fact, the IP protocol suite application layer contains several of these application-level APIs, including HTTP, which is very widely used in mainstream distributed systems.

Still, it’s important to understand TCP, UDP and their differences. Most requests on the Internet are sent using TCP. TCP is:

- Connection-oriented
- Stream-oriented
- Reliable

I’ll explain each of these below.

TCP is known as a connection-oriented protocol. Before any messages are exchanged between applications, TCP uses a 3-step handshake to establish a two-way connection between the client and server applications. The connection stays open until the TCP client calls close to terminate the connection with the TCP server. The server responds by acknowledging the close request before the connection is dropped.

Once a connection is established, a client sends a sequence of requests to the server as a data stream. When a data stream is sent over TCP, it is broken up into individual network packets, with a maximum packet size of 65535 bytes. Each packet contains a source and destination address, which is used by the underlying IP protocol to route the messages across the network.

The Internet is a packet-switched network, which means every packet is individually routed across the network. The route each packet traverses can vary dynamically based on the conditions in the network, such as link congestion or failure. This means the packets may not arrive at the server in the same order they are sent from the client. To solve this problem, a TCP sender includes a sequence number in each packet so the receiver can reassemble packets into a stream that is identical to the order they were sent.

Reliability is needed as network packets can be lost or delayed during transmission between sender and receiver. To achieve reliable packet delivery, TCP uses a cumulative acknowledgement mechanism. This means a receiver will periodically send an acknowledgement packet that contains the highest sequence number of the packets received without gaps in the packet stream. This implicitly acknowledges all packets sent with a lower sequence number, meaning all have been successfully received. If a sender doesn’t receive an acknowledgement within a timeout period, the packet is resent.

TCP has many other features, such as checksums to check packet integrity, and dynamic flow control to ensure a sender doesn’t overwhelm a slow receiver by sending data too quickly. Along with connection establishment and acknowledgments, this makes TCP a relatively heavyweight protocol, which trades off reliability over efficiency.

This is where UDP comes into the picture. UDP is a simple connectionless protocol, which exposes the user’s program to any unreliability of the underlying network. There is no guarantee of in order delivery, or even delivery for that matter. It can be thought of as a thin veneer (layer) on top of the underlying IP protocol, and deliberately trades off raw performance over reliability.

This however is highly appropriate for many modern applications where the odd lost packet has very little effect. Think streaming movies, video conferencing and gaming, where one lost packet is unlikely to be perceptible by a user.

![](img/936a72197720a5158b16541313af61c9_59_0.png)

Figure 3-3. Comparing TCP and UDP

Figure 3-3 depicts some of the major differences between TCP and UDP. TCP incorporates a connection establishment 3-packet handshake (SYN, SYN ACK), and piggybacks acknowledgements (ACK) of packets so that any packet loss can be handled by the protocol. There’s also a TCP connection close phase involving a 4-way handshake that is not shown in the diagram. UDP dispenses with connection establishment, tear down, acknowledgements and retries. Hence applications using UDP need to be tolerant of packet loss and client or server failures and behave accordingly.

## Remote Method Invocation

It’s perfectly feasible to write our distributed applications using low-level APIs that interact directly with the transport layer protocols TCP and UDP. The most common approach is the standardized sockets library - see the brief overview in the sidebar. This is something you’ll hopefully never need to do, as sockets are complex and error prone. Essentially sockets create a bi-directional pipe between two nodes that you can use to send streams of data. There are luckily much better ways to build distributed communications as I’ll describe in this section. These approaches abstract away much of the complexity of using sockets. However sockets lurk underneath, so some knowledge is necessary.

## AN OVERVIEW OF SOCKETS

A socket is one endpoint of a two-way network connection between a client and a server. Sockets are identified by a combination of the nodes’ IP address and an abstraction known as a port. A port is a unique numeric identifier, which allows a node to support communications for multiple applications running on the node. Each {IP Address, port} combination can be associated with an application. This combination forms a unique address that is used by the transport layer to deliver data to the correct application.

Each node can support 65,535 TCP ports and another 65,535 UDP ports. A connection is identified by a combination of source socket and destination socket addresses. Once the connection is created, the client sends data to the server in a stream, and the server responds with results. The sockets library supports both protocols, with the SOCK_STREAM option for TCP, and the SOCK_DGRAM for UDP.

You can write your distributed applications directly to the sockets API, which is an operating system core component. Socket APIs are available in all mainstream programming languages. The sockets library is however a low level, hard to use API, and should be avoided unless you have a real need to write system level code.

Using sockets, in our mobile banking example, the client might request a balance for the user’s checking account. Ignoring specific language issues (and security!!), the client could send a message payload as follows over a connection to the server:

```
{"balance", "000169990"}
```

In this message, “balance” represents the operation we want the server to execute, and “000169990” is the bank account number.

In the server, we need to know that the first string in the message is the operation identifier, and based on this value being “balance”, the second is the bank account number. The server then uses these values to presumably query a database, retrieve the balance and send back the results, perhaps as a message formatted with the account number and current balance, as below:

```
{"000169990", "220.77"}
```

In any complex system, the server will support many operations. In mybank.com, there might be for example “login”, “transfer”, “address”, “statement”, “transactions”, and so on. Each will be followed by different message payloads that the server needs to interpret correctly to fulfill the client’s request.

What we are defining here is an application specific protocol. As long as we send the necessary values in the correct order for each operation, the server will be able to respond correctly. If we have an erroneous client that doesn’t adhere to our application protocol, well, our server needs to do thorough error checking.

The socket library provides a primitive, low level method for client-server communications. It provides highly efficient communications but is difficult to correctly implement and evolve the application protocol to handle all possibilities. There are better mechanisms.

Stepping back, if we were defining the mybank.com server interface in an object-oriented language such as Java, we would have each operation it can process as a method. Each method is passed an appropriate parameter list for that operation, as shown in the example code below.

// Simple mybank.com server interface
public interface MyBank {
    public float balance (String accNo);
    public boolean statement(String month) ;
