
In addition, the Ethernet frame adds an error-checking and correcting section, which enables the receiver at the destination to check what it has received for correctness. If not, the communication system will generate a message back to the original address to resend the damaged frame.

## Ethernet Topologies

The Ethernet protocol is classified as a bus topology that has been implemented across several different network media, including:

- Coaxial cable (IEEE 802.3 – 10BASE-2 or -5)
- Twisted-pair copper cable (IEEE 802.3 – 10/100/1000BASE-T)
- Fiber-optic cable (IEEE 802.3 – 10/100/1000BASE-Fx, Lx or Sx)
- Wireless RF (IEEE 802.11a-h)

All of these topologies may be present in a given utility’s generation and distribution networks. Therefore, the power technician needs to be familiar with wired and wireless IP-networking devices and strategies, as well as fiber optic and radio communications systems.

## Network Control Strategies

When you begin to connect computers and devices together so that they can share resources and data, the issue of who will control the network (and how) comes up very quickly. In some applications, such as developing a book like this one, it is good for the author, artists, and pagination people to be able to share access to text and graphics files, as well as access to devices such as printers. However, in a business network, companies must have control over who can access sensitive information and company resources, as well as when and how much access they should have.

Control of a network can be implemented in two ways:

- As a peer-to-peer network where each computer is attached to the network in a ring or bus fashion and is equal to the other units on the network.
- As a client/server network where dependent workstations, referred to as clients, operate in conjunction with a dedicated master computer called a server.

Figure 13.5 illustrates a typical peer-to-peer network arrangement. In this arrangement, the users connected to the network can share access to different network resources, such as hard drives and printers. In a peer-to-peer network, control of the local unit is fairly autonomous. The nodes in this type of network configuration usually contain local hard drives and printers over which the local computer has control. These resources can be shared at the discretion of the individual user. A common definition of a peer-to-peer network is one in which all the nodes can act as both clients and servers of the other nodes under different conditions.

![](img/2459befd842d6fed299fabe6da3512d0_331_0.png)

## FIGURE 13.5 A Peer-to-Peer Network

Figure 13.6 depicts a typical client/server LAN configuration. In this type of LAN, control tends to be very centralized. The server typically holds the programs and data for its client computers. It also provides security and network policy enforcement.

The major advantages of the client/server networking arrangement include:

- Centralized administration
- Data and resource security
- Network services

When completed, the IPCONFIG /all command will list all current network parameters.

The ARP command will map the network host's IP address to a NIC's MAC address.

Next, the NETSTAT command will be used to identify the current network connections, and the NBTSTAT command will help to resolve the Windows computer names of the other nodes connected on the network.

The NET VIEW command will list the nodes on the LAN.

The TRACERT command will be used to test data packet routing to a remote host and to examine the time required for it to travel between waypoints.

Finally, the PING command will be used to test for responsiveness from a network node.

Keep in mind that the information actually displayed when these utilities run will vary greatly depending on the particular network configuration. The examples provided here will not precisely match the actual results.

## Procedures

## Modifying the Command Prompt Window

Modify the Command Prompt window to increase visibility.

- 1. Boot your PC to the Windows 10 desktop.
- 2. In the Search box located on the taskbar, type the word command, as shown in Figure 13.7.
- 3. Right-click on the Command Prompt option and select Pin To Taskbar to create a shortcut for it directly on the taskbar.
- 4. Click on the Command Prompt shortcut you just created to open the Command Prompt window.
- 5. In a blank area at the top of the Command Prompt window, right-click and then select Properties from the menu that appears.

The first option you will change is the font size. In particular, you will change the size of the font to a size that makes viewing comfortable and is within the capabilities of the monitor being used with your PC.

- 6. Select the Font tab and change the font type to Lucidia Console.
- 7. Click on the Bold fonts checkbox to create a check mark in the box just below the Font list. Your Fonts tab should look similar to Figure 13.8.

![](img/2459befd842d6fed299fabe6da3512d0_334_0.png)

## FIGURE 13.7 Accessing the Command Prompt

![](img/2459befd842d6fed299fabe6da3512d0_334_1.png)

## FIGURE 13.8 Changing the Command Prompt Font Type and Size

## Changing the Color Options

Change the color options to make the command prompt text stand out better.

- 1. In the Properties window, select the Colors tab.
- 2. Click on the radio button next to Screen text, and then move to the color options and select one of the brighter colors toward the end of the color chart. You should choose a bright color that is easy to see, as illustrated in Figure 13.9.

![](img/2459befd842d6fed299fabe6da3512d0_335_0.png)

FIGURE 13.9 Changing the Command Prompt Colors

- 3. Click on the color to obtain a preview of that color against the black background of the Command Prompt window.
- 4. After you finish adjusting your font and colors, click OK.
- 5. The changes you make should take effect immediately. Close the Command Prompt window.

## Using the IPCONFIG Utility

Use the IPCONFIG utility to obtain network configuration information. The IPCONFIG utility permits the observation of the local computer's current IP address and other useful network configuration information.

The `IPCONFIG /all` command will display the complete network information for the computer being used. As illustrated in Figure 13.10, this utility will identify the current network configuration, including the IP address and the physical MAC address.

When using DHCP to provide an IP address, the `ipconfig /release` and `ipconfig /renew` switches can be used to force the DHCP server to withdraw the current IP address lease and obtain a new one.

![](img/2459befd842d6fed299fabe6da3512d0_336_0.png)

- 1. Click on the Command Prompt shortcut that you previously pinned to the taskbar to open a Command Prompt window.
- 2. At the command prompt, type `ipconfig ?` and then press Enter to review the IPCONFIG options. You can scroll up to review buffered parts of the text that may have cycled off screen.
- 3. At the command prompt, type `ipconfig /all` and press Enter.
- 4. Record the requested information from the results for your PC in the designated areas of Table 13.1.

**TABLE 13.1 LAN Information**

|  |  |
|---|---|
|  |  |
|  |  |

## Using the ARP Utility

The Address Resolution Protocol (ARP) utility can be used to identify addressing information by examining the contents of the ARP caches on either the client or the server. It is primarily used to map IP addresses to physical MAC addresses belonging to active network connections.

- 1. At the command prompt, type arp and then press Enter. Review the usage notes for the ARP utility.
- 2. At the command prompt, type arp -a and then press Enter. This will display information similar to Figure 13.11.

![](img/2459befd842d6fed299fabe6da3512d0_337_0.png)

FIGURE 13.11 Mapping IP Addresses to MAC Addresses

- 3. Record the IP address of the host computer, as shown in the Interface line, on the following line:
- 4. View the IP and physical MAC addresses of the network connections.

## Using the NETSTAT Utility

The netstat -e command will be used to display the number of data packets transmitted and received, as well as the number of errors generated during the data transfer. The netstat -r command will display a list of all current connections as well as indicate which ones are active.

- 1. At the command prompt, type netstat ? and press Enter. Review the usage notes for the NETSTAT utility.
