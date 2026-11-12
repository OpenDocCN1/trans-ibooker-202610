
The following shows the more common commands used to configure a DHCP server on a Cisco IOS device:

```
IOS(config)# service dhcp
IOS(config)# ip dhcp pool pool_name
IOS(dhcp-config)# network network_number [mask | /prefix-length]
IOS(dhcp-config)# domain-name domain
IOS(dhcp-config)# dns-server address [address2... address8]
IOS(dhcp-config)# default-router address
IOS(dhcp-config)# lease {days [hours] [minutes] | infinite}
IOS(dhcp-config)# exit
IOS(config)# ip dhcp excluded-address low-address [high-address]
```

In the example,

- **service dhcp** Enables the DHCP server and/or relay features on your IOS device. Without this command, the IOS device will not act as a server or relay agent. As of IOS 12.2, this command is enabled by default.
- **ip dhcp pool** Creates a name for the DHCP server address pool and places you in DHCP pool subcommand mode.
- **network** Specifies the subnet network number and mask of the DHCP address pool. Also, *prefix-length* specifies the number of bits that make up the address prefix. The prefix is an alternative way of specifying the network mask of the client. The prefix length must be preceded by a forward slash (/).
- **domain-name** Specifies the domain name to be assigned to the client.
- **dns-server** Specifies the IP address of a DNS server that is available to a DHCP client. One IP address is required; however, you can specify up to eight IP addresses in one command line.
- **default-router** Specifies the IP address of the default gateway to be assigned to the DHCP client.
- **lease** Specifies the duration of the lease. IP addresses are leased to hosts, typically on a temporary basis. When a client’s address lease time reaches half the life of the lease, it will attempt to renew the existing addressing information it obtained previously. If unsuccessful in its renewal attempt, the device will continue to attempt renewal at periodic intervals. The default lease duration is one day, but other parameters can be configured for the pool, such as a TFTP server address for VoIP phones, but this is beyond the scope of this book. You can set the lease to infinite, which would mean the lease does not expire.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_759_0.png)

**EXAM TIP** For the Cisco CCT and CCNA exams, know that a DHCP server assigns IP address settings to a requesting host. The CCNA exam expects you to know how to configure DHCP—on Cisco IOS DHCP servers, addresses are defined in a pool. If you are defining network numbers for your pool, be sure to exclude the network number and the directed broadcast address from the pool. IP addresses are leased to hosts, and when a client’s lease time reaches half the lease length, it will attempt to renew the existing address information.

- **ip dhcp excluded-address** Specifies the IP addresses that the DHCP server should not assign to DHCP clients based on the network numbers you’ve defined for the DHCP pool or pools. These addresses are already used in the subnet, such as static addresses assigned to the switches and routing device. If you are defining network numbers for your pool, make sure you exclude the network number and the directed broadcast address from the pool. Notice that this command is not part of the pool configuration, but is a Global Configuration command.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_759_1.png)

**NOTE** Typically, Cisco routers are configured as DHCP servers for small networks, such as branch offices. In large corporate or campus networks, a dedicated DHCP server or servers are usually set up in the data center and/or distribution layers of the network.

To view the DHCP pool information, use the **show ip dhcp pool** command:

```
IOS# show ip dhcp pool
Pool 1:
  Utilization mark (high/low)      : 85 / 15
  Subnet size (first/next)         : 24 / 24
  Total addresses                  : 254
  Leased addresses                 : 11
  Pending event                    : none
1 subnet is currently in the pool:
Current index      IP address range          Leased addresses
10.1.1.12           10.1.1.1 - 10.1.1.254      11
```

With this command, you can view the total number of available addresses, the configured address range, and the number of currently leased addresses.

To view the current IP addresses assigned to DHCP clients, use the **show ip dhcp binding** command:

```
IOS# show ip dhcp binding
Bindings from all pools not associated with VRF:
IP address      Client-ID/               Lease expiration      Type
                 Hardware address/
                 User name
10.1.1.1        0063.6973.636f           Mar 29 2014 04:36 AM   Automatic
```

This command lists the IP addresses assigned to the clients: the IP-to-MAC address bindings.

When assigning IP addresses to clients, IOS will first ping the IP address to ensure that it hasn’t been assigned to anyone else; additionally, IOS will examine its ARP table to ensure that the IP address isn’t in use by any other device. To view these conflicts, use the **show ip dhcp conflict** command:

```
IOS# show ip dhcp conflict
IP address      Detection method         Detection time         VRF
172.16.1.32     Ping                     Feb 16 1998 12:28 PM
172.16.1.64     Gratuitous ARP           Feb 23 1998 08:12 AM
```

If an address conflict is detected, the address is removed from the DHCP pool and an administrator must manually resolve the problem. Use the **clear ip dhcp conflict** command to clear a conflicted address from the list. Optionally, you can use the **debug ip dhcp server** command to troubleshoot problems with the operation of DHCP on an IOS device.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_761_0.png)

**EXAM TIP** If an address conflict is detected, the address is removed from the DHCP pool and an administrator must manually resolve the problem. Be familiar with the output of the **show ip dhcp conflict** command.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_761_1.png)

**VIDEO 13.02.** The digital resources that accompany this book contain a multimedia demonstration of configuring and verifying a DHCP server on a Cisco router.

## DHCP Relay Configuration

In larger networks, networking devices that need addresses via DHCP may not reside on the same broadcast domain as the DHCP server and therefore are unable to reach a DHCP server. In most companies, the DHCP servers are centralized at the distribution layer or in the data center. Therefore, by default, the DHCP servers will not see the DHCP requests that the clients are advertising; routers don’t forward broadcast messages to solve this issue.

The solution to this problem is to use the IOS feature *DHCP relay*: the Cisco router acts as a relay agent with the DHCP REQUEST message. The process is shown in Figure 13-4. In the figure, when the router receives the broadcasted addressing request from the client, it changes it to a unicast message, with the router as the source and the DHCP server as the destination. The agent includes the network number to help the DHCP server choose the correct addressing pool. All messages between the agent and the DHCP server are delivered using unicast. From the client’s perspective, it thinks it’s actually communicating locally with the DHCP server.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_762_0.png)

Figure 13-4 DHCP agent relay process

To configure the router as a DHCP relay agent, use the following:

```
Router(config)# service dhcp
Router(config)# interface g0/0
Router(config-if)# ip helper-address DHCP_server_IP_address
```

The service dhcp command is enabled by default—you need to execute this only if the DHCP service has been manually disabled. You then navigate to the interface you want to configure to listen for DHCP messages. The ip helper-address command is used to configure the router as a DHCP relay agent and defines the DHCP server to forward DHCP messages to. You can configure this command multiple times if you have redundant DHCP servers.

If you want to verify that the DHCP relay operation is configured properly and identify the address of the DHCP server that the DHCP messages are forwarded to, you can use the show ip interface command:

```
VAN-R1#show ip interface
GigabitEthernet0/0 is up, line protocol is up (connected)
  Internet address is 12.0.0.1/8
  Broadcast address is 255.255.255.255
  Address determined by setup command
  MTU is 1500 bytes
  Helper address is 14.0.0.10
  Directed broadcast forwarding is disabled
(output omitted for briefness)
```

![](img/0eafdcebe219ef3756f0e57bcf1eb997_763_0.png)

**VIDEO 13.03.** The digital resources that accompany this book contain a multimedia demonstration of configuring and verifying DHCP relay on a Cisco router.

## Exercise 13-2: Configuring DHCP Services

In this exercise you will configure VAN-R1 as a DHCP server to provide IP addresses of 12.100.0.1 up to 12.100.0.50. We are using the same network topology for this exercise as the last exercise.

- 1. Ensure that all physical components such as routers and switches are connected as per the network diagram, and then power on the devices.
- 2. Connect your workstation to the console port of VAN-R1. Then launch PuTTY, choose Serial Connection, and set the correct COM port. Then click Open.
