
## EtherChannel Configuration

As mentioned, you should make sure that all interfaces in the channel are configured identically (configuring interface properties was discussed in Chapter 9); otherwise, a channel may not form. Here is the configuration to set up an EtherChannel:

```
Switch(config)# interface type [slot_#]/port_#
Switch(config-if)# channel-group group_# mode mode
Switch(config-if)# port-channel load-balance {dst-ip | dst-mac | src-dst-ip | src-dst-mac | src-ip | src-mac}
```

The group_# specifies the channel group to which the interface belongs, which can be from 1 to 6 (remember that you can have up to six EtherChannels on your switch). The mode can be one of those listed in Table 10-6. When using PAgP, one side needs to be configured as desirable and the other side as desirable or auto; or you can configure both sides to be on. When using LACP, one side needs to be active and the other side can be active or passive.

| Mode | Protocol | Description |
|---|---|---|
| auto | PAgP | Passively listens for PAgP queries from a Cisco device configured with either desirable or on. By default, the interface is not part of a channel. |
| desirable | PAgP | Generates PAgP queries to form a channel, but by default is not part of a channel. |
| on | PAgP | Generates PAgP queries and assumes the port is part of a channel. |
| active | LACP | Enables a channel if the other side responds to its LACP messages. |
| passive | LACP | Passively listens for LACP messages to form a channel from an active port. |

Table 10-6 EtherChannel Modes

![](img/0eafdcebe219ef3756f0e57bcf1eb997_612_0.png)

**NOTE** If you want to treat this as a layer 3 interface, on a router for example, and assign an IP address to it, the port channel must be designated as **interface port-channel** *group_#*. 

![](img/0eafdcebe219ef3756f0e57bcf1eb997_613_0.png)

**EXAM TIP** Remember for the CCNA exam that if one side of a PAgP EtherChannel is set to auto for the mode, the other needs to be set to either on or desirable. The active and passive modes are used only to establish an EtherChannel using LACP.

Here’s a configuration of SwitchA forming an EtherChannel to SwitchB using PAgP:

```
SwitchA(config)# interface g1/0/1
SwitchA(config-if)# channel-group 1 mode auto
SwitchA(config-if)# exit
SwitchA(config)# interface g1/0/2
SwitchA(config-if)# channel-group 1 mode auto
SwitchA(config-if)# exit
```

Because the auto mode is used on SwitchA for PAgP, SwitchB must use either a mode of on or desirable, like this:

```
SwitchB(config)# interface g1/0/4
SwitchB(config-if)# channel-group 1 mode on
SwitchB(config-if)# exit
SwitchB(config)# interface g1/0/5
SwitchB(config-if)# channel-group 1 mode on
SwitchB(config-if)# exit
```

## Exercise 10-3: Working with EtherChannel

In this exercise, you will configure an EtherChannel connection between the VAN-SW1 and VAN-SW2 switches in the Vancouver office.

1. Ensure that both switches are powered on.
2. If the crossover cables are still connected from the previous exercise, remove them.
3. Use crossover cables to connect ports 1 and 2 between the two switches.
4. Console into VAN-SW1 and enter the following commands:
5. View the EtherChannel summary information by running the **show etherchannel summary** command. Record the following information:

    Group: ________________
    Protocol: ________________
    Ports: ________________

6. View the status of the port-channel 1 interface with the **show interfaces port-channel 1** command. Note that the command could also be abbreviated as **show interfaces po1**.
7. Switch to VAN-SW2 and enter the following commands to place interfaces 1 and 2 in an EtherChannel:

    ```
    VAN-SW1>enable
    VAN-SW1#config term
    Enter configuration commands, one per line. End with CNTL/Z.
    VAN-SW1(config)#interface g1/0/1
    VAN-SW1(config-if)#channel-group 1 mode auto

    VAN-SW1(config-if)#interface g1/0/2
    VAN-SW1(config-if)#channel-group 1 mode auto
    ```

    ```
    VAN-SW2>enable
    VAN-SW2#config term
    VAN-SW2(config)#interface g1/0/1
    VAN-SW2(config-if)#channel-group 1 mode on

    VAN-SW2(config-if)#interface g1/0/2
    VAN-SW2(config-if)#channel-group 1 mode on
    ```

8. View the EtherChannel summary information by running the **show etherchannel summary** command. Record the following information:

    Group: ________________
    Protocol: ________________
    Ports: ________________

9. View the status of the port-channel 1 interface with the **show interfaces port-channel 1** command.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_615_0.png)

**VIDEO 10.03.** The digital resources that accompany this book contain a multimedia demonstration of configuring EtherChannel on your switch.

## Chapter Review

VTP is a Cisco-proprietary protocol that transmits VLAN information across trunk ports. Switches must be in the same domain to share messages. There are three modes for VTP: client, server, and transparent. Server and transparent switches can add, change, and delete VLANs, but server switches advertise these changes. Clients can accept updates only from server switches. There are three VTP messages: advertisement request, subset advertisement, and summary advertisement. Servers generate summary advertisements every five minutes on trunk connections. The configuration revision number is used to determine which server switch has the most current VLAN information. VTP pruning is used to prune off VLANs that are not active between two switches, but it requires switches to be in server and/or client mode.

On the Cisco switch, use the **vtp domain** command and **vtp server|client|transparent** commands to configure VTP. The default mode is server. To configure a VTP password, use the **vtp password** command.

BPDUs are used by STP to learn about other neighboring switches. These are generated every two seconds as multicasts. When running STP, a root switch is elected—the one with the lowest switch or bridge ID. The switch ID is composed of a priority and the switch’s MAC address. Each switch chooses a root port to reach the root switch—the one with the lowest accumulated path cost. Each segment has one port on one switch that becomes a designated port, which is used to forward traffic to and from the segment. This is typically the port on the switch with the lowest accumulated path cost. There are five port states: blocking (20 seconds), listening (15 seconds), learning (15 seconds), forwarding, and disabled. PortFast puts a port immediately into forwarding mode and should be used only on non-switch ports. PVST+ has an instance of STP running per VLAN—this is proprietary to Cisco but standardized by IEEE with MSTP.

RSTP reduces convergence to a few seconds by having switches determine valid alternate root ports and backup designated ports that they can use when topology changes take place. PVST+ with RSTP is enabled with the **spanning-tree mode rapid-pvst** command.

EtherChannels bundle layer 2 connections between devices, creating a single logical port from STP’s perspective. Load balancing can then be performed on the ports in the channel. PAgP or LACP is used to form the channel. No more than eight interfaces can be part of a channel.

If your CPU and/or port utilization is high, you may have a layer 2 loop. Typically you should use a protocol analyzer and look for multiple copies of the same frame in your frame captures.

### Quick Review

### VLAN Trunking Protocol

- VTP is used to share VLAN information to ensure that switches have a consistent VLAN configuration.
- VTP has three modes: server (can make and accept changes and propagates changes), transparent (can make changes, ignores VTP messages), and client (accepts changes from servers and doesn’t store them in NVRAM). The default mode is server.
- VTP messages are propagated only across trunks. For a switch to accept a VTP message, the domain name and optional password must match. There are three VTP messages: advertisement request (client or server request), subset advertisement (server response to an

