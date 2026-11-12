In the middle of the output is **System restarted** followed by how the IOS device was last started. In this example, the device had its power turned on. The last line of this output is the configuration register value. This value determines how the router will boot up, including how it will find its IOS and its configuration file. The bootup process of the router is discussed in more depth in [Chapter 6](#).

![](bbox=[0.14, 0.26, 0.24, 0.32])

**EXAM TIP** Be familiar with the output of the **show version** command, including what is displayed, such as IOS version; the uptime; the amount of RAM, NVRAM, and flash; the type and number of interfaces; and the configuration register value. You can also view the last method used to boot up the IOS device, such as someone executing the **reload** command or power-cycling the device.

# Interface Information

The CCT certification objectives include objective 4.11, which is to identify and correct common network problems at layer 1 and layer 2. Layer 1 problems would be related to identifying physical connection issues such as an unconnected device cable. Layer 2 issues would be related to the layer 2 protocol configuration such as a serial link having HDLC or PPP configured properly on the serial interfaces.

In this section you will learn a number of commands that you can use to identify layer 1 and layer 2 problems, to determine whether an interface is configured for full or half duplex, and to verify the speed of the interface.

You typically will notice any problems with an interface when you enable it, because a status message states whether the interface is up (enabled with no problems) or down (unable to enable due to configuration errors). In Ethernet, a status message could be issued if the wrong cable type (crossover versus straight-through) is used, there is a mismatch in the speed or duplex settings, or a bad cable is used. This section will discuss how to diagnose these problems with the **show interfaces** command.