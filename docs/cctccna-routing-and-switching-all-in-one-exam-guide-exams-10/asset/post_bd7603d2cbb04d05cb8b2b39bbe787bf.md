
| Command | Description |
|---|---|
| hostname VAN-SW1 | Displays the MAC addresses associated with each port. |
| no shutdown | Configures port security to learn only one MAC address for a port. |
| ip default-gateway 12.0.0.1 | Sets the device name. |
| show mac address-table | Configures port security for the MAC address of the currently connected device. |
| switchport port-security mac-address sticky | Enables an interface. |
| switchport port-security maximum 1 | Sets the default gateway on the device. |

## Answers

- 1. C. The System LED will be amber if the switch has experienced a malfunction during startup. If the system boots without error, the System LED will display green.
- 2. D. When in the Duplex display mode, each port LED will display green if it is running in full-duplex mode, but the LED is off if the port is running in half duplex.
- 3. A. Enabling the interfaces is not asked for during the System Configuration Dialog script. Interfaces are enabled by default on Cisco switches.
- 4. C. The ip default-gateway command is a global configuration mode command.
- 5. B. An IP address is assigned to the switch so that you can use telnet or SSH to administer the switch remotely.
- 6. To view the CAM table, also known as the MAC address table, you would use the show mac address-table command.
- 7. A. If the switch receives a frame that contains a destination address that is not located in the MAC address table, the switch will flood the frame to all ports on the switch.
- 8. A. Port security is used to prevent unauthorized access to a LAN.
- 9. **C.** When configuring port security, you can configure the port security feature to authorize the MAC address of the system using the port by using the sticky option (**switchport port-security mac-address sticky**). This prevents you from having to input each MAC address on each port.
- 10. **D.** The port states that it is administratively down; this means that the port was disabled by an administrator.
- 11. **A.** The **switchport mode trunk** command enables trunking on a Cisco switch. Keep in mind that you would run this command after setting the encapsulation mode with **switchport trunk encapsulation dot1q**.
- 12. **C.** The **switchport access vlan** command assigns an interface to a VLAN. You would run this command after placing the interface in access mode.

## Performance-based Answers

The following are the answers to the performance-based questions in this chapter.

- 1. This question is tricky as it is two questions in one. First, it is testing your knowledge of assigning an IP address to the switch, and the default gateway. Second, it is an IP subnetting question because you also need to ensure the commands you use are using an IP address in the correct IP range.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_555_0.png)

```text
VAN-SW1(config)#interface vlan 1
VAN-SW1(config-if)#ip address 192.168.35.98 255.255.255.224
VAN-SW1(config-if)#no shutdown
VAN-SW1(config-if)#exit
VAN-SW1(config)#ip default-gateway 192.168.35.65
```

```text
VAN-SW1(config)#interface g0/0
VAN-SW1(config-if)#ip address 192.168.35.92 255.255.255.224
VAN-SW1(config-if)#no shutdown
VAN-SW1(config-if)#exit
VAN-SW1(config)#ip default-gateway 192.168.35.65
```

```text
VAN-SW1(config)#interface vlan 1
VAN-SW1(config-if)#ip address 192.168.35.98 255.255.255.224
VAN-SW1(config-if)#ip default-gateway 192.168.35.65
```

## LA-SW1 Configuration Commands

```text
VAN-SW1(config)#interface vlan 1
VAN-SW1(config-if)#ip address 192.168.35.92 255.255.255.224
VAN-SW1(config-if)#no shutdown
VAN-SW1(config-if)#exit
VAN-SW1(config)#ip default-gateway 192.168.35.65
```

- 2. On the real Cisco exams, you will need to drag an item on the left side and drop it onto the matching item on the right side. In this question you need to match the switch command with its description. The following is the correct answer for this performance question.

| Command | Description |
|---|---|
| hostname VAN-SW1 | Sets the device name. |
| no shutdown | Enables an interface. |
| ip default-gateway 12.0.0.1 | Sets the default gateway on the device. |
| show mac address-table | Displays the MAC addresses associated with each port. |
| switchport port-security mac-address sticky | Configures port security for the MAC address of the currently connected device. |
| switchport port-security maximum 1 | Configures port security to learn only one MAC address for a port. |

## Advanced Switching Features

In this chapter, you will

- Learn how to set up and manage the VLAN Trunking Protocol
- Learn about the Spanning Tree Protocol
- Learn about the Rapid Spanning Tree Protocol
- Learn how to manage EtherChannels

This chapter is the last chapter on layer 2 functions. Most larger networks implement redundancy to protect data in case of failures; redundancy can involve multiple wide area network (WAN) connections, multiple paths in your layer 3 network, and/or multiple paths in your layer 2 network. This chapter focuses on layer 2 redundancy and the issues involved with layer 2 loops, including two features that are commonly used to solve these problems: the Spanning Tree Protocol and EtherChannels.

## VLAN Trunking Protocol

The VLAN Trunking Protocol (VTP) is a proprietary Cisco protocol used to share virtual local area network (VLAN) configuration information between Cisco switches on trunk connections. VTP enables switches to share and synchronize their VLAN information, which ensures that your network has a consistent VLAN configuration.

Assume, for instance, that your network has two switches, and you need to add a new VLAN. You could easily accomplish this by adding the VLAN manually on both switches. However, this process becomes more difficult and tedious if you have 30 switches to deal with. In this situation, you could make a mistake in configuring the new VLAN on one of the switches, such as giving it the wrong VLAN identifier, or you might forget to add the new VLAN to one of the 30 switches. VTP can take care of these issues. With VTP, you can add the VLAN on one switch and have this switch propagate the new VLAN, via VTP messages, to all of the other switches in your layer 2 network, causing them to add the new VLAN also.

This is also true if you modify a VLAN’s configuration or delete a VLAN—VTP can verify that your VLAN configuration is consistent across all of your switches. VTP can even perform consistency checks with your VLANs to make sure that all the VLANs are configured identically.

Components of a VLAN can include a VLAN number, name, and type. So, for example, if you have a VLAN number of 1 and a name of “admin” on one switch, but the name “administrator” is used on a second switch for the same VLAN 1, VTP checks for and fixes the configuration mismatches.

VTP messages will propagate only across trunk connections, so you will need to set up trunking between your switches in order to share VLAN information via VTP. VTP messages are propagated as layer 2 multicast frames by layer 2 devices. Therefore, if a router separates two of your switches, the router will not forward the VTP messages from one of its interfaces to another because it is a layer 3 device.

For VTP to function correctly, you must associate your switch with a VTP domain. A domain is a group of switches that have the same VLAN information applied to them. Basically, a VTP domain is similar to an autonomous system, which is used by some routing protocols (autonomous systems and routing protocols are introduced in Chapter 11). A switch can belong only to a single VTP domain. VTP domains are given names, and when switches generate VTP messages, they include the VTP domain name in their messages. When a VTP message is received by a switch, the VTP message is only processed if the VTP domain in the message is the same as the VTP domain name configured on the switch. In other words, a switch in one VTP domain will ignore VTP messages from switches in other VTP domains. The following sections cover the components and messages that VTP uses, as well as some of the advantages that it provides, such as pruning.
