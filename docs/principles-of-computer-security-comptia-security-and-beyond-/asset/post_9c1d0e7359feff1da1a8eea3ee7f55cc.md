
2. Why is physical access required to perform password recovery on a Cisco router?

3. Why is physical access required to perform password recovery on a Cisco switch?

## Key Term Quiz

Use the terms from the list to complete the sentences that follow.

- booting
- configuration
- recovery

1. The process of hacking into a Cisco router or switch can also be described as password ________.

2. Getting full control of a Linux system requires physical access, and the process involves interrupting the ________ process.

3. Hacking into a switch is made possible by renaming the ________ file.

## Chapter 9
Network Fundamentals

## Lab Exercises

- 9.01 Switch Configuration
- 9.02 Router Configuration
- 9.03 Passwords and SSH

Lab Analysis

Key Term Quiz

Think of switches as the local streets of a city, connecting entities (such as homes, buildings banks, coffeehouses, and supermarkets) in the same vicinity. These entities are like the end devices on a network. End devices are the sources or destinations of traffic and have IP addresses. End devices are connected by switches and include desktops, laptops, printers, cameras, mobile devices, and so on. Just like the streets of a city allow you to easily go from a bank to a coffeehouse, switches allow network traffic (for example, a document) to go from your computer to a network printer. Just like a set of streets can lead to another part of a city, switches connect to other switches, extending the network’s reach. Of course, a network must have a switch that connects to a router, to take traffic destined for other networks off the network and bring traffic from other networks onto that network.

Think of routers as the highways that connect different cities and states together. Unlike switches, routers don’t connect devices together. Routers connect networks together. Routers also connect to other routers that are connected to other routers that are connected to other routers, and so forth. This allows traffic from one network to be sent to any interconnected

network via any number of routers and allows the return traffic to make its way back to the device that initiated the traffic on the originating network. Just like highways allow you to go from Rochester to Staten Island, routers allow your network traffic to reach devices on other networks that could be in the same building, another state, or another country or continent.

60 MINUTES

## Lab Exercise 9.01: Switch Configuration

When a PC sends an Ethernet frame into a switch, the switch examines the destination MAC address in the frame and then checks to see if it knows which port a device with that destination MAC address is connected to. If the switch knows where the device with that MAC address is, the switch sends the frame out of just that corresponding port. If the switch doesn’t know where the destination MAC address is, the switch floods the frame out of all ports, except the port on which the frame originated.

### Note

When referring to routers and switches, the terms port and interface are very often used interchangeably. Technically speaking, though, a port is a physical unit that a connector is plugged into, while an interface is a logical representation of that port, with a name like GigabitEthernet0/0/0, for example.

So now the obvious question is, how does the switch learn where MAC addresses are in the first place? The switch starts off knowing nothing, but as frames are sent into the switch, the switch starts learning. Refer to Figure 9-1.

![](img/6e921d997751f0a0f87e5170ac9657a1_259_0.png)

## FIGURE 9-1 Two switches

Switch1 SAT

|  |  |
|---|---|
| 1 | A |
| 2 | B |
| 3 | C |
| 4 | D |
| 5 | E |
| 5 | F |
| 5 | G |
| 5 | H |

Switch2 SAT

|  |  |
|---|---|
| 1 | E |
| 2 | F |
| 3 | G |
| 4 | H |
| 5 | A |
| 5 | B |
| 5 | C |
| 5 | D |

In this figure, if HostA sends a frame for HostB into Port1 on Switch1, the switch will associate the MAC address of HostA with the port on which it was heard through a table in memory known by various names, including the source address table (SAT), the content addressable memory (CAM) table, and the MAC address table.

Since the switch doesn’t know where HostB is, it floods the frame out of all the ports, except the one the frame originated on. When HostB replies, the frame goes into the switch, and the switch learns that HostB can be found on Port2. The switch then adds the MAC address of HostB and the port on which it was heard into its source address table as well.

The logic works the same for switches that are directly connected together. Each switch maintains its own source address table. You’ll notice that Hosts E, F, G, and H are known to Switch2 on Ports 1, 2, 3, and 4, respectively. However, as far as Switch1 goes, Hosts E, F, G, and H are all accessible through Port5, which connects to Switch2.

This lab exercise will introduce you to Cisco’s Packet Tracer, a free simulation of everything networking! However, if you have the hardware necessary to perform this lab (see the optional lab materials listed), as well as all the other related labs in this chapter, it would be great to use the real gear. If you’re using actual switches and PCs, ignore the Packet Tracer–specific instructions. All the commands, though, will be the same.

If you want to take a break or revisit a certain portion of the lab exercise, you can save your work by clicking File | Save and then giving the file a name. Then, when you’re ready to continue, you can just reopen the .pkt file from your Cisco Packet Tracer directory, named after the version number, inside a directory with the same name as your username inside of C:\Users. Cisco Packet Tracer 7.3.0 is the current version at the time of publication.

### Note

Press ENTER after every command.

![](img/6e921d997751f0a0f87e5170ac9657a1_261_0.png)

Step 1 This sequence of instructions for setting up Packet Tracer changes from time to time. If what you’re seeing doesn’t match these instructions exactly, make the appropriate modifications.

- a. Go to https://www.netacad.com/courses/packet-tracer.
- b. Click the link that reads “Simply enroll into Introduction to Packet Tracer course to download the latest version of Packet Tracer.”
- c. Click “Sign up today!” and select English.
- d. Fill in the required info (including an e-mail address to get the activation link) in the Enroll Now section on the right of the page and then click Submit.
- e. Fill out the required details and click Register.
- f. Follow the prompts to log in with your credentials and then enter the verification code sent to your e-mail.
- g. Enter the requested information on the next screen and click Create Account.
- h. Click Launch Course and enter your credentials.
- i. Click either Next or End tour.
- j. Feel free to complete some, none, or all parts of the course as you see fit.
- k. Click Resources in the top menu and select Download Cisco Packet Tracer.
- l. On the bottom of the My NetAcad page, click Packet Tracer.
- m. Select the correct version for your system.
- n. When the download completes, install Packet Tracer. Keep all default settings and respond to any security prompts accordingly.
- o. Launch Packet Tracer and provide your Cisco Network Academy credentials. You should now see the Cisco Packet Tracer application.

![](img/6e921d997751f0a0f87e5170ac9657a1_262_0.png)

Step 2 Refer to Figure 9-2 as you construct your topology, in this step, with switches, PCs, and cables.

![](img/6e921d997751f0a0f87e5170ac9657a1_263_0.png)

## FIGURE 9-2 Topology for this lab exercise

- a. Click the Network Devices icon (located under the time counter in the lower left). In the row below that, click the Switches icon (second from the left).
- b. Drag two 2960 switches (first icon on the left in the bottom-middle pane) to the blank main window.
- c. Click the End Devices icon (to the right of Network Devices). From the pane to the right, drag four PCs (first icon) to the topology, placing two below each switch.
- d. Click the Connections icon (two to the right of End Devices) and connect two PCs to each switch with a Copper Straight-Through cable (third from the left in the bottom-middle pane). After you click the cable icon to select it, click a PC and switch in your topology. Use FastEthernet0 on each PC and use FastEthernet0/1 and FastEthernet0/2 on each switch. After a few seconds, you’ll see a green upward-facing triangle (the switch side will show an orange circle first) on each side of both PC-to-switch connections, indicating that there is a live connection between each PC and its switch.
