
## Loopback Plugs

A loopback plug can be connected to a port to verify that the port is able to send and receive a signal without the signal leaving the system. Loopback plugs are available for different types of interfaces such as an RJ-45 Ethernet interface, a T1 link, and a 56K link. The loopback plug is a specially designed connector that has the transmit pins of the connector (pins 1 and 2 for UTP cabling) connected to the bidirectional pins on the same connector (pins 4 and 5 for UTP cabling).

Let’s look at an example of creating a loopback plug to use for a T1 interface. You will need a crimper, an RJ-45 connector, and a small piece of Ethernet cable (about 3 to 4 inches in length). Then follow these steps to make your own T1 loopback plug:

- 1. Cut a small section of Ethernet cable (about 3 or 4 inches).
- 2. Remove the outer jacket of the cable.
- 3. Position the RJ-45 connector clip away from you. The left side is pin 1 and the right side is pin 8 in the connector.
- 4. Using the orange wire color (it could be any wire), slide one end of the wire into pin 1 of the RJ-45 connector, and then bend the wire to connect the other end of the wire into pin 4 of the connector.
- 5. Using the blue wire color (it could be any wire), slide one end of the wire into pin 2 of the RJ-45 connector, and then bend the wire and connect the other end of the wire into pin 5 of the connector.
- 6. With the wires in place, put the RJ-45 connector into the crimper and squeeze to crimp the wire.

Once the loopback plug is created, you can connect it to the T1 port on your router and you should receive an indication of a link or loopback with an LED (depends on the interface).

## Types of Cisco Device Cables

You should be familiar with several different types of cables for the Cisco CCT certification exam.

### Console Cable

The console cable is typically a light-blue cable that has a serial port (RS-232) connector on one end and an RJ-45 connector on the opposite end. This cable is used to connect your computer to the Cisco device so you can locally configure the device. As mentioned earlier, if you do not have a serial port on your computer, as is the case with most laptops, you will need a USB-to-serial adapter so that you can connect the console cable to your USB port instead.

### Ethernet Cable

You will use a straight-through Ethernet cable to connect the workstation to the switch or to connect the router to the switch. You may also need crossover cables to connect similar devices, such as two switches.

### Serial Cable

Use a back-to-back serial cable to connect two routers using the routers’ serial ports. There are a number of different types of serial ports, so make sure you have the correct types of cables for those ports. Figure 6-14 displays two different types of serial interfaces on a router and the appropriate cabling.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_274_0.png)

Figure 6-14 Serial cables to connect to the serial ports

### Switch Stack Cable

Many of the enterprise-level switches support the StackWise feature, which enables you to connect the switches together as a manageable unit so you can centrally configure all switches from a single point. To connect the switches in a stack, you need to use a StackWise cable, as shown in Figure 6-15.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_275_0.png)

Figure 6-15 StackWise cable to connect switches in a stack

## Exercise 6-1: Identifying Cisco Device Components

In this exercise you will identify the components on a Cisco device by using the following diagram and locating the following parts (not all components are necessarily present):

|  |  |
|---|---|
| Console port | Serial port |
| Ethernet ports | Auxiliary port |
| Integrated CSU/DSU |  |

![](img/0eafdcebe219ef3756f0e57bcf1eb997_275_1.png)

## Understanding Memory Types

Now let’s take a look at the Cisco software components, starting with the different types of memory found on a Cisco device and the type of information that is stored in that memory. This is important stuff for the Cisco CCT exam, so be sure to study it well.

### Read-Only Memory (ROM)

The software in ROM cannot be changed unless you actually swap out the ROM chip on your Cisco device. ROM is nonvolatile—meaning that when you turn off your device, the contents of ROM are not erased. ROM contains the firmware necessary to boot up your router and typically has the following three components:

- **Power-on self-test (POST)** Performs tests on the device’s hardware components.
- **Bootstrap program** Brings the router up and determines how the IOS image and configuration files will be found and loaded.
- **ROM Monitor (ROMMON)** A mini-OS that enables you to perform low-level testing and troubleshooting; for instance, ROMMON is used during the password recovery procedure. To abort the router’s normal bootup procedure of loading IOS, press ctrl-break to enter ROMMON mode. The prompt in ROMMON mode is either > or rommon>, depending on the router model.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_276_0.png)

**EXAM TIP** POST performs self-tests on the hardware. The bootstrap program brings the router up and finds an IOS image. ROMMON contains a mini–operating system used for low-level testing and debugging. The Mini-IOS is a stripped-down version of the IOS used for emergency booting of a router and is referred to as RXBOOT mode. All of these components are stored in ROM.

### Flash Memory

Flash is a form of nonvolatile memory, like ROM, meaning that when you turn the router off, the information stored in flash is not lost. Routers store their IOS image in flash, but other information can also be stored here, such as a secondary configuration file. Note that some lower-end Cisco routers actually run the IOS directly from flash (not RAM). Flash is slower than RAM, a fact that can create performance issues. Figure 6-16 displays flash memory being inserted into the compact flash slot.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_277_0.png)

Figure 6-16 Flash memory on a Cisco router

### Volatile RAM (VRAM)

Volatile RAM (VRAM) on a Cisco device is like RAM in your computer in the sense that it loses its contents when you shut the system down. On a router, volatile RAM, sometimes just referred to as RAM, contains the running IOS image, the active configuration file (known as running config), any tables (including routing, ARP, and other tables), and internal buffers for temporarily storing information such as interface input and output buffers and logging messages. When you turn off your router, everything in RAM is erased, including the items mentioned, such as your running config.

### Nonvolatile RAM (NVRAM)

NVRAM is like flash in that its contents are not erased when you turn off your router. It is slightly different, though, in that it uses a battery to maintain the information when the Cisco device is turned off. Routers (and switches) use NVRAM to store the configuration files, including the startup config, which is applied to the device during bootup after the IOS is loaded.

## Exercise 6-2: Identifying Memory Types

In this exercise you will match the memory term with the description of what it does.

| Term | Description |
|---|---|
| A. Flash memory | ___ Contains the startup configuration file |
| B. Volatile RAM | ___ Contains the running configuration file |
| C. ROM | ___ Contains the IOS image files |
| D. Nonvolatile RAM | ___ Contains bootstrap and ROMMON programs |

## Administering the Cisco Device

Once you have the device installed, you can boot up and configure the device. Before we get into the configuration (see Chapter 7), you should know that you can administer the device in several ways, discussed in this section.

### Using the CLI

The first and most often used method of administrating a Cisco device is to use the command line interface (CLI). The CLI is a command environment similar to a Windows command prompt or Linux terminal where you must enter commands into the system to apply configuration changes. The Cisco CCT and CCNA certification exams expect you to know how to use the CLI to apply configuration changes.

You can use the CLI for local or remote administration. With local administration, you have connected your laptop to the console port of the device and will use the CLI to administer the device. The term “local” here means that you are in close physical proximity to the device and can connect to the console port. After the device has been configured and has an assigned IP address, you can remotely administer the device from a
