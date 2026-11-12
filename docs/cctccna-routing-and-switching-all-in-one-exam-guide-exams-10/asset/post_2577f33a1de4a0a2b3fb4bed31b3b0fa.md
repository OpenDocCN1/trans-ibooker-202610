
```text
hostname Switch
enable secret 5 $1$.N.L$t4q9Jw5DTffPTPE.KkKNX/
enable password enable
line vty 0 15
  password remotepass
no snmp-server

interface Vlan1
  no shutdown
  ip address 12.0.0.2 255.0.0.0
!
interface FastEthernet0/1
  .
  .
  .
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
end
[0] Go to IOS command prompt without saving this config.
[1] Return back to the setup without saving this config.
[2] Save this configuration to nvram and exit.
Enter your selection [2]: 2
```

At the end of the script, enter 2 to accept and activate your changes, as well as save the configuration to NVRAM. (Entering 0 aborts the script, and entering 1 starts the script over, remembering what you just entered as the defaults for the questions you were just asked.)

![](img/0eafdcebe219ef3756f0e57bcf1eb997_491_0.png)

**TIP** One problem with this script is that once you answer a question—correctly or incorrectly—there is no way of going back to the question. To abort the script, press CTRL-C and start over. To run the script from the command line interface (CLI) without rebooting the switch, go to privileged EXEC mode and execute the **setup** command.

## Basic Settings

You can change the configuration settings of your Cisco switch at any time using IOS commands. In this section, you will learn how to change the device name and set an IP address and default gateway on the switch.

### Naming the Device

The first configuration change I typically make with a switch is to change the hostname. If you change the hostname, the prompt within the CLI changes to the hostname, making it easy to know which device you are configuring. Use the hostname global configuration command to modify the name of your device, as shown here:

```text
Switch>enable
Switch#config term
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname VAN-SW1
VAN-SW1(config)#
```

### Assigning an IP Address

After you change the hostname, you may want to assign an IP address to your switch. Note that a switch is a layer 2 device and does not need an IP address to perform its job of learning and forwarding frames on the network. By assigning an IP address to a switch, however, you can remotely connect to the switch using HTTP, telnet, or SSH. You can remotely manage it instead of depending on connecting through a local console port.

Switches work a bit differently from routers in the sense that you do not assign IP addresses to the ports of the switch. The devices that are connected to the ports, such as computers or IP phones, have their IP addresses assigned. To assign an IP address to a switch, you first navigate to a special VLAN interface and then assign the IP to the VLAN of the switch, as shown here:

```text
VAN-SW1>enable
VAN-SW1#config term
Enter configuration commands, one per line.  End with CNTL/Z.
VAN-SW1(config)#interface vlan1
VAN-SW1(config-if)#ip address 12.0.0.2 255.0.0.0
VAN-SW1(config-if)#no shutdown
%LINK-5-CHANGED: Interface Vlan1, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
VAN-SW1(config-if)#
```

Notice in this code example that from global configuration mode, you navigate to the vlan1 (default VLAN) by using the **interface** command. Once at the interface prompt, you use the **ip address** command to assign an IP address and subnet mask to the switch. You then must enable the interface by using the **no shutdown** command. You will learn more about VLANs in the next chapter.

### Setting Up a Default Gateway

Once you’ve assigned the IP address and subnet mask, you can remotely administer the switch as long as you are on the same network as the switch. If you want to administer the switch from a different network, you must configure a default gateway so that the switch knows how to send data off the network. To configure a default gateway on the switch, use the following commands:

```text
VAN-SW1>enable
VAN-SW1#config term
Enter configuration commands, one per line.  End with CNTL/Z.
VAN-SW1(config)#ip default-gateway 12.0.0.1
```

Notice that we navigated to global configuration mode and then used the **ip default-gateway** command to set the default gateway.

### Exercise 9-2: Configuring a Switch

In this exercise, you will apply basic configuration settings to the VAN-SW1 and LA-SW1, which are located in the Vancouver office and the Los Angeles office, respectively. The network diagram for this exercise and the remaining exercises is shown in the following illustration. Notice that this is a basic network topology, with three networks—the 12.0.0.0, 13.0.0.0, and 14.0.0.0 networks that we use for most of the exercises. Because we are using public IP addresses, it is a good practice to ensure that your lab environment is not connected to any production network and is isolated.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_494_0.png)

Note that ComputerA is connected to port 5 of VAN-SW1, while ComputerB is connected to port 15 of VAN-SW1. ComputerC is connected to port 5 of LA-SW1, while ComputerD is connected to port 15 on LA-SW1.

- 1. Ensure all routers and switches are booted up.

#### Configuring VAN-SW1

- 2. Connect to the console port of VAN-SW1 with PuTTY.
- 3. Change the name of the switch with the following commands:

```text
Switch>enable
Switch#config term
Enter configuration commands, one per line. End with CNTL/Z.
Switch(config)#hostname VAN-SW1
VAN-SW1(config)#
```

- 4. Assign an IP address to the switch with the following commands:

```text
VAN-SW1(config)#interface vlan1
VAN-SW1(config-if)#ip address 12.0.0.2 255.0.0.0
VAN-SW1(config-if)#no shutdown
```

- 5. Assign a default gateway setting to the switch with the following commands:

```text
VAN-SW1(config-if)#exit
VAN-SW1(config)#ip default-gateway 12.0.0.1
```

- 6. Verify that the IP address has been configured by viewing the status on the VLAN1 interface:

```text
VAN-SW1(config)#exit
%SYS-5-CONFIG_I: Configured from console by console
VAN-SW1#show interface vlan1
Vlan1 is up, line protocol is up
  Hardware is CPU Interface, address is 00d0.ff58.9cde (bia 00d0.ff58.9cde)
  Internet address is 12.0.0.2/8
  MTU 1500 bytes, BW 100000 Kbit, DLY 1000000 usec,
  reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
(output cut for briefness)
VAN-SW1#
```

#### Configuring LA-SW1

Using the commands you entered for VAN-SW1 as a guide, configure the LA-SW1 switch by following these steps:

- 7. Connect to the console port on LA-SW1 and press ENTER in PuTTY to refresh the prompt.
- 8. Change the name of the switch to LA-SW1.
- 9. Assign the IP address of 14.0.0.2 to the switch.
- 10. Assign a default gateway setting of 14.0.0.1 to the switch.
- 11. Verify that the IP address has been configured by viewing the status on the VLAN1 interface.

## MAC Address Table

You’ll recall that one of the three main functions of a switch is to learn which devices—that is, MAC addresses—are associated with which interfaces or ports on the switch. You also learned that this information is stored in the MAC address table, or CAM table. On newer switches, you can view the MAC address table by using the **show mac address-table** command:

```text
VAN-SW1>enable
VAN-SW1#show mac address-table
```

Mac Address Table:

| Vlan | Mac Address | Type | Ports |
|------|-------------|------|-------|
| 1 | 0050.0fdd.d03a | DYNAMIC | Gig1/0/1 |
| 1 | 0090.0c9e.76a8 | DYNAMIC | Gig1/0/5 |

In this example, you can see that two MAC addresses are stored in the CAM table: one associated with Fa0/1 and the other, Fa0/5. Remember that these entries are automatically created based on the switch receiving traffic through these ports.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_496_0.png)

**EXAM TIP** Some exam questions require you to be familiar with the output of the **show mac address-table** command. Also remember that if a destination MAC address is not in the table (unknown), the switch will flood the frame to all ports on the switch.

To clear dynamically learned entries from the CAM table, use the **clear mac address-table** command from privileged EXEC mode.
