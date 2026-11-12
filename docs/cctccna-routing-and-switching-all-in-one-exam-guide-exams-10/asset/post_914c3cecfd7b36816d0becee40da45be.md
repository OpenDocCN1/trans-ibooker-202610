
## Configuring TCP/IP on Systems

You can modify the TCP/IP configuration settings on your system. The steps are different depending on the operating system you are running, but the following sections show the general steps to configure Windows, Linux, and macOS systems.

### Windows 10

To change the TCP/IP settings on your Windows system use these steps:

- 1. Click the Start button, and then type **control**. In the Best Match section of the Start button you will see Control Panel.
- 2. Choose Control Panel | Network and Internet | Network and Sharing Center.
- 3. Click the Change Adapter Settings link on the left.
- 4. Right-click Ethernet and choose Properties.
- 5. Select Internet Protocol Version 4 (TCP/IPv4) and choose Properties.
- 6. In the Properties dialog box, notice that, by default, the radio button Obtain An IP Address Automatically is selected, which means that the system is configured as a DHCP client:

### Internet Protocol Version 4 (TCP/IPv4) Properties

You can get IP settings assigned automatically if your network supports this capability. Otherwise, you need to ask your network administrator for the appropriate IP settings.

Obtain an IP address automatically

Use the following IP address:

Obtain DNS server address automatically

Use the following DNS server addresses:

Validate settings upon exit

- 7. To configure the IP address statically, choose Use The Following IP Address: and then enter the IP address, subnet mask, and default gateway setting for the system.
- 8. To configure the system to use a specific DNS server, choose the Use The Following DNS Server Addresses and then enter the IP address of the DNS server you want to use.
- 9. Choose OK and close all windows.

### Linux

There are a few different ways you can use to modify your IP address on Linux systems depending on the distribution. For years, administrators used the ifconfig command to change their IP address with the following syntax:

```
ifconfig eth0 12.0.0.11 netmask 255.0.0.0
```

Where:

**eth0** is the interface or network card you are modifying.
**12.0.0.11** is the IP address you want to use.
**netmask 255.0.0.0** is the subnet mask.

You can also change your IP address in Linux using the graphical interface. To do this, follow these steps:

- 1. On your Linux system, in the top right corner, you will see a network icon. Right-click the network icon and choose Edit Connections:

![](img/0eafdcebe219ef3756f0e57bcf1eb997_193_0.png)

- 2. Select your connection and then choose the Settings icon at the bottom (looks like a gear icon).
- 3. Choose the IPv4 Settings tab.
- 4. In the Method drop-down list, choose Manual.
- 5. Click the Add button to add the IP address, subnet mask, and default gateway you want to use.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_194_0.png)

- 6. Enter the IP address of the DNS Servers and then click Save.
- 7. Close all windows.

### macOS

Modify your macOS IP address using the following steps:

- 1. From the Apple menu, choose System Preferences.
- 2. Choose Network.
- 3. Select your network interface on the left.
- 4. Click the Advanced button in the bottom right corner.
- 5. Choose the TCP/IP tab.

- 6. To configure an IP address manually, from the Configure IPv4 drop-down list, choose Manually. Then enter your IP address, subnet mask, and router address (default gateway).

### Verify IP Address Settings

Once you have configured your TCP/IP settings, it is a good idea to verify that the IP address settings have taken effect. You can do this in each of the operating systems with a quick command.

- **Windows** Open a command prompt and type **ipconfig** to view basic TCP/IP settings such as the IP address, subnet mask, and default gateway. If you want to view all your TCP/IP settings including DHCP lease times and DNS server settings, use the **ipconfig /all** command.
- **Linux** Depending on the version of Linux you are running, you can either use the **ifconfig** command to view your TCP/IP settings or use the Network Manager command **nmcli** to check out your IP settings.
- **macOS** Use the **ifconfig** command to verify your TCP/IP settings.

### Understanding the Pathway of Communication

Before we finish up on the basics of TCP/IP, you should understand the communication pathway of communication between two systems on a TCP/IP network. The following illustration shows the steps that occur when ComputerB sends data to ComputerD:

![](img/0eafdcebe219ef3756f0e57bcf1eb997_196_0.png)

- 1. The command **ping 13.0.0.10** is entered at ComputerB to invoke communication.
- 2. ComputerB compares its IP address and subnet mask against the IP address that is being pinged to determine whether the system is on the same network. Because the network IDs are different, IP determines the systems are on different networks (note this is actually done at the binary level with a process called ANDing), as shown here:
```
12.0.0.10
255.0.0.0
13.0.0.10
```
- 3. ComputerB retrieves the default gateway settings from the TCP/IP settings, as that is the address it needs to send the data to in order to send data to a different network.
- 4. ComputerB then ARPs the IP address of the default gateway to obtain the MAC address of the default gateway.
- 5. The ARP reply message is returned to ComputerB with the MAC address of the default gateway.
- 6. ComputerB sends the data to the router with the following header information:
- 7. The router receives the layer 2 frame and then reads the layer 3 header to determine where the packet is headed. The router sees that the packet is destined for 13.0.0.10, so it checks to see if it has a route in the routing table. It does, and it realizes it just needs to send data out the 13.0.0.1 interface to reach the destination.
- 8. To send the data, the router needs to know the MAC address of 13.0.0.10, so it sends out an ARP request message.
- 9. The router receives the ARP reply containing the MAC address of ComputerD.
- 10. The router sends the data to ComputerD with the following header information:

```
Layer 2 Header:
    Source MAC: 2222.2222.2222
    Destination MAC: AAAA.5555.5555
Layer 3 Header:
    Source IP: 12.0.0.10
    Destination IP: 13.0.0.10
```

```
Layer 2 Header:
    Source MAC: BBBB.6666.6666
    Destination MAC: 4444.4444.4444
Layer 3 Header:
    Source IP: 12.0.0.10
    Destination IP: 13.0.0.10
```

The important point here is to notice that the layer 2 header is modified each step along the way to move the frame to the next step. But the layer 3 header is never modified, because it contains the network address to route the data to the correct network.

### Exercise 4-6: Analyzing the Pathway to Communication

In this exercise, you will analyze the following network diagram and identify characteristics of the packet as it travels from ComputerA to ComputerD.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_198_0.png)

- 1. Looking at the diagram, record the layer 2 and layer 3 header information that the packet will have at the point circled with a 1.
  Layer 2 Source Address: _________________________
  Layer 2 Destination Address: _________________________
  Layer 3 Source Address: _________________________
  Layer 3 Destination Address: _________________________
- 2. Looking at the diagram, record the layer 2 and layer 3 header information that the packet will have at the point circled with a 2.
  Layer 2 Source Address: _________________________
  Layer 2 Destination Address: _________________________
Layer 3 Source Address: ________________
Layer 3 Destination Address: ________________
- 3. Looking at the diagram, record the layer 2 and layer 3 header information that the packet will have at the point circled with a 3.
Layer 2 Source Address: ________________
Layer 2 Destination Address: ________________
Layer 3 Source Address: ________________
Layer 3 Destination Address: ________________

### Chapter Review

In this chapter you learned about the fundamentals of TCP/IP and what makes this protocol so common in today’s networking market. The following list summarizes what you learned:

- The IP address and subnet mask are the most important configuration settings and must be specified correctly in order to communicate on a TCP/IP-based network. Next in importance is the default gateway, which specifies where to route packets if you are communicating outside the local network.
- The Dynamic Host Configuration Protocol (DHCP) automatically configures a workstation with the correct TCP/IP settings, relieving you of the burden of manually configuring every workstation.
- TCP/IP addressing involves a strong knowledge of the IP address, subnet mask, network classes, and special reserved addresses. (You should memorize each network class for the exam.)
- The most important portions of TCP/IP as it relates to your CCT and CCNA exams are the TCP/IP configuration concepts. You need these to configure workstations with TCP/IP. The concepts include the IP address, the subnet mask, and the default gateway.

With a strong understanding of the material presented in this chapter, you will have no problems with any TCP/IP-related questions on your exam. Not only is the material presented here important for each exam, but it will also be important after you ace the exam and continue on to a career as a networking professional.
