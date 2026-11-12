BPDU is incremented by the port cost of the incoming port. This helps the layer 2 device figure out which ports should be root and designated ports.

**port security** A switch feature that enables you to lock down switch ports based on the MAC address or addresses associated with the interface, preventing unauthorized access to a LAN.

**PortFast** A feature that enables a port to be always placed in a forwarding state—this is true even when STP is running and the root and designated ports are going through their different states. So, when STP is running, PortFast ports on the same switch can still forward traffic among themselves, somewhat limiting your STP disruption. PortFast should be configured only on ports not connected to other switches, such as ports that have PCs, servers, and routers connected to them.

**power-on self test (POST)** A process that performs hardware tests when a Cisco device is booting up. These tests can include interfaces, lines, and memory components. For many components, if a failure occurs, the Cisco device will fail to boot up.

**PPP Authentication Protocol (PAP)** The simplest, but least secure, of PPP’s authentication protocols. During the authentication phase, PAP will go through a two-way handshake process. In this process, the source sends its username and password, in clear text, to the destination. The destination compares this information with a list of locally stored usernames and passwords. If it finds a match, the destination sends back an accept message. If it doesn’t find a match, it sends back a reject message.

**presentation layer** The sixth layer of the OSI Reference Model, which is responsible for defining how information, such as text, graphics, video, and/or audio information, is presented to the user in the interface being used.

**private IP address** RFC 1918 is a document that was created to address the shortage of IP addresses. When devices want to communicate with each other, each device needs a unique address. RFC 1918 created a private address space that any company can use internally. These addresses include 10.0.0.0/8, 172.16.0.0/16–172.31.0.0/16, and 192.168.0.0/24–