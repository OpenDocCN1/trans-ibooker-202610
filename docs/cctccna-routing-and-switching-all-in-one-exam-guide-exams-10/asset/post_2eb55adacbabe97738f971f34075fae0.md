
14. C. Network Address Translation (NAT) is responsible for translating a private IP address in a packet to a public IP address so that it can travel over the Internet.

15. A. The DHCP network service is responsible for assigning IP address information to network devices so that they can communicate on the network.

16. D. The DNS service converts the FQDNs to IP addresses. If you cannot ping the FQDN, but you can ping the IP address, it could mean a problem with DNS converting the FQDN to an IP address. This typically means either the FQDN does not exist in DNS or you are configured to use the wrong DNS server.

## Performance-based Answers

The following are the answers to the performance-based questions that are common on Cisco exams.

1. The following is the order of the wires for the 568B standard.
- White wire / orange stripe
- Orange wire
- White wire / green stripe
- Blue wire
- White wire / blue stripe
- Green wire
- White wire / brown stripe
- Brown wire

2. The following shows the correct type of cabling to connect different types of devices together. Remember that a crossover is used if the devices are considered similar devices (the exception being a router connecting to a computer).

![](img/0eafdcebe219ef3756f0e57bcf1eb997_94_0.png)

![](img/0eafdcebe219ef3756f0e57bcf1eb997_94_1.png)

![](img/0eafdcebe219ef3756f0e57bcf1eb997_94_2.png)

![](img/0eafdcebe219ef3756f0e57bcf1eb997_94_3.png)

![](img/0eafdcebe219ef3756f0e57bcf1eb997_94_4.png)

![](img/0eafdcebe219ef3756f0e57bcf1eb997_94_5.png)

## OSI Reference Model

In this chapter, you will
- Learn about the functions of the OSI Reference Model layers
- Learn about encapsulation and de-encapsulation

Before you can successfully configure Cisco switches and routers, you must understand the basic networking concepts outlined in this chapter and advanced concepts discussed in later chapters. The Open Systems Interconnection (OSI) Reference Model is the best place to start, since it will help you understand how information is transferred between networking components. Of the seven layers in the OSI Reference Model, you should understand how the bottom four layers of the OSI model function, because most networking components function at these layers. This chapter covers how traffic is generally moved between network components, and it uses the OSI Reference Model to illustrate the encapsulation and de-encapsulation process.

## Layers of the OSI Reference Model

In the early 1980s, the International Organization for Standardization (ISO) defined a standard for manufacturers of networking components that would enable these networking components to communicate in dissimilar environments. The product of the standard is demonstrated in the seven layers of the OSI Reference Model, shown in Figure 2-1: application, presentation, session, transport, network, data link, and physical. Each layer is responsible for performing a specific process or role.

|  |  |
|---|---|
| Layer 7 | Application |
| Layer 6 | Presentation |
| Layer 5 | Session |
| Layer 4 | Transport |
| Layer 3 | Network |
| Layer 2 | Data link |
| Layer 1 | Physical |

Figure 2-1 Layers of the OSI Reference Model

![](img/0eafdcebe219ef3756f0e57bcf1eb997_96_0.png)

**NOTE** A good mnemonic to use to remember the OSI Reference Model layers is “All people seem to need data processing” (application, presentation, session, transport, network, data link, and physical).

The functions of the first three layers—application, presentation, and session—are known as the *upper layers* of the OSI model, and they are typically controlled by the application software. These layers are normally not controlled or modified by a network administrator. Network administrators can use knowledge of the transport, network, data link, and physical layers to help troubleshoot network communication. These four layers, known as the *lower layers* of the OSI model, are the layers you should feel comfortable with as a network professional.

Each layer of the OSI model is responsible for communicating with the layers directly above and below it, receiving data from or passing data to its neighboring layers. For example, the presentation layer will receive information from the application layer, format it appropriately, and then pass it to the session layer. The presentation layer will never deal directly with the transport, network, data link, or physical layers. The same idea is true for all layers with regard to their communication with other layers.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_97_0.png)

**CAUTION** Understanding the functions of each of the OSI Reference Model layers is very important when it comes to troubleshooting network components and network communication. Once you understand these functions and the troubleshooting tools available to you at the various layers of the model, troubleshooting network-related problems will be much easier.

Network communication starts at the application layer of the OSI model (on the sending system) and works its way down through the layers to the physical layer. The information then passes along the communication medium to the receiving system, which works its way back up the layers, starting at the physical layer, as shown in Figure 2-2. (Be sure to refer to this figure frequently when reading through this section.)

![](img/0eafdcebe219ef3756f0e57bcf1eb997_98_0.png)

Figure 2-2 How packets move through the layers of the OSI model

When you’re thinking of two devices communicating, such as two computers, it is important to understand that whatever action is done at one layer of a sending computer is undone at the same layer on the receiving computer. For example, if the presentation layer compresses the information on the sending computer, then the data is uncompressed on the receiving computer.

Let’s look at the layers from the point of view of two computers that are sending data between each other. Computer1 and Server1 are exchanging data on the network. Computer1 is the sending computer, and Server1 is the receiving computer, as shown in Figure 2-3. The data exchange starts with Computer1 sending a request to Server1 in the application layer. At Server1, the data moves back up through the layers to the application layer, which passes the data to the appropriate application or service on the system.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_99_0.png)

Figure 2-3 Identifying the function of each layer of the OSI model

![](img/0eafdcebe219ef3756f0e57bcf1eb997_99_1.png)

**EXAM TIP** The CCT exam is sure to test your knowledge of the OSI model and each of its layers, so be familiar with this for the exam! You should also note that the data has special terms assigned to it once it reaches different layers—at layer 4, the data is called a *segment*; at layer 3, the data is called a *packet*; and at layer 2, the data is called a *frame*.

## Layer 7: The Application Layer

The application layer running on the sending system (Computer1) is responsible for initiating the actual request. This could be any type of networking request—a web request using a web browser (HTTP), an e-mail delivery request using Simple Mail Transport Protocol (SMTP), or a file system request using the network client redirector software. On the receiving system, the application layer would be responsible for passing the request to the appropriate application or service on that system. In our example, we will assume that you are sitting at Computer1 and you have typed the address of Server1 into your web browser to create an HTTP request.

There are many, many examples of protocols that run at the application layer. The most common are HTTP, telnet, Secure Shell (SSH), File Transfer Protocol (FTP), Post Office Protocol 3 (POP3), and Simple Mail Transfer Protocol (SMTP).

![](img/0eafdcebe219ef3756f0e57bcf1eb997_100_0.png)

EXAM TIP For the exams, remember that the top (seventh) layer of the OSI Reference Model, the application layer, provides the protocols and services that enable systems to initiate a request (sending system) or service a request (receiving system). Examples of application layer protocols include telnet, FTP, HTTP (for web browsing), SSH, POP3, and SMTP.

## Layer 6: The Presentation Layer

After the request is made, the application layer passes the data down to the presentation layer, where it is formatted so that the data (or request) can be interpreted by the receiving system. When the presentation layer receives data from the application layer, it makes sure the data is in the proper format—if it is not, the presentation layer converts the data accordingly. On the receiving system, when the presentation layer receives network data from the session layer, it makes sure the data is in the proper format and once again converts it if it is not.

Formatting functions that could occur at the sending system’s presentation layer include compression, encryption, and ensuring that the character code set can be interpreted on the other side. For example, if we choose to compress our data from the application that we are using, the application layer will pass that request to the presentation layer, but the presentation layer does the compression. At the receiving end, this data
