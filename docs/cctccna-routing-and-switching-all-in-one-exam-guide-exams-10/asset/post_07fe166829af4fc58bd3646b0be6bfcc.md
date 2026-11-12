
devices are working. You will see these commands in more detail in Chapter 7.

In this scenario, you have connected a computer to interface 5 on the switch and you want to verify that the interface is working okay. Use the following commands to verify your interfaces are up and running:

- **show ip interface brief** Use this great **show** command to get a list of all of the interfaces on the device and take a quick look at their statuses. You can determine whether the interface is up and running and view the IP address assigned to the interface.
- **show interfaces** Use this command to view a list of all the interfaces and detailed specifications on the interfaces.
- **show interfaces g0/0** Use this command to view the details of a specific interface.
- **show controllers s0/3/0** Use the **show controllers** command on serial interfaces to view information such as the DCE and DTE details.

## Chapter Review

In this chapter you learned about the many Cisco hardware and software features that are available with Cisco routers and switches. You learned about the different families of Cisco devices, such as the Cisco Nexus and Cisco Catalyst Series devices. You also learned how to identify the model of the Cisco device you are servicing and where to locate its serial number.

You also learned about the different types of ports included on a Cisco device, such as the console port, auxiliary port, Ethernet ports, and serial ports used for WAN connections. You learned about installing the Cisco device into a rack and using the SFP+ port to connect a transceiver of your choice.

You learned about the different types of memory that exist on a Cisco device such as ROM, VRAM, NVRAM, and flash memory. Finally, you learned about the boot process for a Cisco device when you power on the device.

## Quick Review

### Working with Cisco Devices and Software

- Cisco offers several product lines, such as Cisco Nexus Series switches and the Cisco Catalyst Series switches.
- The model and serial number are typically located on a sticker on the back of the Cisco device.
- The console port is used for local administration, while the auxiliary port is used to connect a modem to the router as a backup administration line.
- Each LAN port is identified by the type of port and an index number, such as FE0/0 for the first Fast Ethernet port in slot 0 or G0/3 as the third Gigabit port in slot 0.

### Understanding Memory Types

- ROM memory stores the POST routines, the bootstrap program, and the ROMMON program used when troubleshooting boot issues.
- Flash memory stores the IOS images.
- Volatile RAM stores the running configuration and different tables in RAM, such as the routing table.
- Nonvolatile memory stores the startup config.

### Administering the Cisco Device

- You can locally administer the Cisco device by connecting to the console port of the device using a console cable.
- You can remotely connect to the device over the network by using telnet or SSH to perform administration on a device.
- On some devices, such as wireless access points, you can use a web interface for administration.

### Starting the Cisco Device

- When you boot the Cisco device, the POST runs first, and then the bootstrap program is loaded, which locates the IOS image to load. Finally, the startup configuration is applied after the IOS is loaded.
- If there is no startup configuration, the device will prompt you to run the System Configuration Dialog, which is a wizard-style interface that prompts you for the configuration information.
- Use the **boot system** command to specify which IOS image to boot from.

## Service-Related Tasks for Technicians

- As a service technician, you must know how to connect to a Cisco device via the console port to perform local administration.
- Know how to use programs on your laptop, such as a text editor and terminal emulation software, to connect to the Cisco device.
- Know common troubleshooting commands such as **ping**, **ipconfig**, and **tracert** to troubleshoot communication issues.

## Questions

The following questions will help you measure your understanding of the material presented in this chapter. Read all the choices carefully because there may be more than one correct answer. Choose the correct answer(s) for each question.

1. What type of memory on the Cisco devices stores the POST routines?
   A. NVRAM
   B. VRAM
   C. Flash
   D. ROM
2. You are reviewing the ports on a router with your customer, who asks what is the purpose of the AUX port. How would you respond?
   A. It’s used to telnet into the router.
   B. It’s used to connect a modem to the router.
   C. It’s used to SSH into the router.
   D. It’s used as an additional Ethernet connection.
3. You are administering a switch for a customer and need to reference the third Gigabit Ethernet port on the switch. What label identifier is used to reference the port?
   A. Eth1/3
   B. FE1/3
   C. Gig1/3
   D. Ethernet1/3
4. Where is the startup configuration file stored on the Cisco device?
   A. ROM
   B. VRAM
   C. NVRAM
   D. Flash
5. Your router has two serial ports used to connect to the WAN. You are connecting the external CSU/DSU to the second serial port. What is the label identifier to reference that port?
   A. S0/1
   B. S0/0
   C. G0/0
   D. G0/1
6. You need to make some changes to a customer’s switch configuration. You have connected the console cable to the console port on the switch and the serial port on your laptop. What program would you use to administer the device? (Choose two.)
   A. Notepad
   B. PuTTY
   C. Ping
   D. Tracert
   E. Tera Term
7. You have made a number of configuration changes to the Cisco device. What type of memory is used to store the running configuration?
   A. ROM
   B. Flash
   C. NVRAM
   D. VRAM
8. Your customer has called, indicating he is having trouble getting out to the Internet. You arrive at the customer’s site and plug your laptop into the network. What command would you use to verify that the router is up and running?
   A. ipconfig
   B. ping
   C. arp
   D. netstat
9. What type of memory is used to store the IOS image file for the Cisco device?
   A. Flash
   B. VRAM
   C. ROM
   D. NVRAM

## Performance-based Questions

1. Using the following exhibit, match the different commands with their definitions.

- show inventory
- nslookup
- ping
- traceroute
- show version

- Used to verify a system or device is up and running
- Used to view IOS and interface information
- Used to verify the pathway that a packet takes to reach its destination
- Used to view hardware components installed in the device
- Used to query the DNS server and find out an IP address of an FQDN

2. Place the steps that the Cisco device takes during the startup process in order by writing the step found on the left into the appropriate box on the right.

### Steps

![](img/0eafdcebe219ef3756f0e57bcf1eb997_302_0.png)

- Startup config is applied
- IOS is loaded from flash
- Bootstrap program loaded and executed
- Configuration register value read to determine location of IOS image and configuration file
- Device loads and runs POST

## Answers

1. D. Read-only memory (ROM) is used to store the POST routines, bootstrap program, and the ROMMON program.
2. B. You can use the AUX port to connect a modem so that you can dial into the router if there are networking issues, such as the network interfaces are down.
3. C. The correct identifier is Gig1/3: Gigabit Ethernet interfaces are referenced beginning with Gig, G, or GigabitEthernet, followed by the index number of the slot, and then the index number of the interface.
4. C. The startup configuration is stored in nonvolatile RAM (NVRAM), which retains the configuration even when the device is powered off.
5. A. The serial port is referenced with S or Serial, followed by the slot and port numbers. Slot and port numbers on routers start at 0, so in this example, the second serial port would be S0/1.
6. B, E. You can use any terminal emulator, such as PuTTY or Tera Term. Both programs are available online as a free download.
7. D. The running configuration is lost when the device is powered down, so it is stored in volatile RAM (VRAM).
8. B. The ping command can be used to send test messages to a device by its IP address or DNS name and verify that the router is up and
