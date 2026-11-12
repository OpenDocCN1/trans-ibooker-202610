
limit from which remote workstation the administrator is allowed to administer the router.

- 1. Ensure that you have completed the previous exercise before continuing with this exercise.
- 2. Connect your workstation to the console port of VAN-R1. Then launch PuTTY, choose Serial connection, set the correct COM port, and choose Open.
- 3. Press ENTER in the PuTTY window to get a prompt on your Cisco device.
- 4. Access VAN-R1’s privileged EXEC mode and create a standard access list numbered 1 that permits the entire 12.0.0.0/8 IP address range and applies this to the VTY ports as the only authorized IP addresses for remote administration:
- 5. Create another standard access list, numbered 2, that permits only traffic from the IP address of 14.0.0.5 and the IP address of 14.0.0.1, and denies traffic from all other traffic from the 14.0.0.0 network. Apply the access list to the WAN interface for inbound:

```
VAN-R1>enable
Password: labenablesecret
VAN-R1#config term
VAN-R1(config)#access-list 1 permit 12.0.0.0 0.255.255.255
VAN-R1(config)#line vty 0 15
VAN-R1(config-line)#access-class 1 in
VAN-R1(config-line)#exit
```

```
VAN-R1(config)#access-list 2 permit 14.0.0.5 0.0.0.0
VAN-R1(config)#access-list 2 permit 14.0.0.1 0.0.0.0
VAN-R1(config)#access-list 2 deny 14.0.0.0 0.255.255.255
VAN-R1(config)#interface g0/0
VAN-R1(config-if)#ip access-group 2 out
VAN-R1(config-if)#exit
```

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1010_0.png)

**NOTE** We applied the access list to the internal network card instead of the one connected to the WAN so that Open Shortest Path First (OSPF) packets coming from the LA-R1 can be received. Any traffic from the 14.0.0.0 network will not be delivered into the 12.0.0.0 network unless it is coming from 14.0.0.1 or 14.0.0.5.

- 6. Go to the computer with the IP address of 14.0.0.5 and try to ping 12.0.0.5. Were you successful? _______
- 7. Go to another computer on the 14.0.0.0 network and try to ping 12.0.0.5. Were you successful? _______

## Extended Numbered ACLs

Extended IP ACLs are much more flexible in what you can match on than standard ACLs. Extended ACLs can match on all of the following information:

- Source *and* destination IP addresses
- TCP/IP protocol (IP, TCP, UDP, ICMP, and so on)
- Protocol information, such as port numbers for TCP and UDP, or message types for ICMP

The following sections cover the configuration and use of extended numbered IP ACLs.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1011_0.png)

**EXAM TIP** For the CCNA exam, remember that extended IP ACLs can filter on source and destination IP addresses, protocol, and protocol data such as source and destination ports. Because extended ACLs are very specific in what they filter, Cisco recommends placing them as close as possible to the source that is being filtered.

## Command Syntax

Here is the generic command to configure an extended numbered IP ACL:

```
Router(config)# access-list {100-199 | 2000-2699} {permit | deny}
    IP_protocol
    source_address source_wildcard_mask
    [protocol_information]
    destination_address destination_wildcard_mask
    [protocol_information] [log]
```

As you can see from this command, the configuration of an extended ACL is more complicated than that of a standard one. Extended IP numbered ACLs can use list numbers in the ranges 100–199 and 2000–2699. After the action (permit or deny) comes the IP protocol that you want to match on. This is the first major difference between an extended ACL and a standard one. These IP protocols include the following: ip, icmp, tcp, gre, udp, igrp, eigrp, igmp, ipinip, nos, and ospf. If you want to match on any IP protocol—TCP, UDP, ICMP, and so on—use the ip keyword for the protocol. If Cisco doesn’t have a name for the IP protocol you want to specify, use the number of the protocol instead, such as 6 for TCP.

The second major difference is that you must specify both the source and destination addresses and their respective wildcard masks. With a standard ACL, you can specify only the source address, and the wildcard mask is optional. Depending on the IP protocol, you may be able to add protocol information for the source and/or destination. For example, TCP and UDP enable you to specify both source and destination port numbers, and ICMP enables you to specify ICMP message types. As with standard ACLs, you can log messages to the console or a syslog server with the log parameter.

Once you have created your extended numbered IP ACL, you must activate it on your router’s interface with the ip access-group command. Note that this is the same configuration used with a standard ACL. Once you activate the ACL, the router will begin filtering traffic on the interface.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1012_0.png)

**EXAM TIP** Be very familiar with the general syntax of an extended ACL statement: you may have to configure an extended ACL on the exam.

## TCP and UDP

Use the following syntax to configure an extended ACL for TCP or UDP:

```
Router(config)# access-list {100-199 | 2000-2699} {permit | deny}
    {tcp | udp} source_address source_wildcard_mask
    [operator source_port_#]
    destination_address destination_wildcard_mask
    [operator destination_port_#]
    [established] [log]
```

After specifying the action (**permit** or **deny**), you configure the IP protocol: **tcp** or **udp**.

## Operators

With TCP and UDP, you can specify the source, destination, or both source and destination port numbers or names. To specify how to perform the match, you must configure an operator. The operator tells the router how to match on the port number(s) or names. Table 17-5 lists the valid operators for TCP and UDP ACL entries. Note that these operators apply only to TCP and UDP connections. Other IP protocols do not use them. If you omit the port number or name, the ACL looks for a match on all TCP or UDP connections.

| Operator | Explanation |
|---|---|
| lt | Less than |
| gt | Greater than |
| neq | Not equal to |
| eq | Equal to |
| range | Range of port numbers |

Table 17-5 TCP and UDP Operators

## Ports Numbers and Names

For TCP and UDP connections, you can list either the name or the number of the port. For example, if you wanted to match on telnet traffic, you could use either the keyword telnet or the number 23. Table 17-6 lists some of the most common port names and numbers for TCP connections.

| Port Name | Command Parameter | Port Number |
|---|---|---|
| FTP Data | ftp-data | 20 |
| FTP Control | ftp | 21 |
| Telnet | telnet | 23 |
| SMTP | smtp | 25 |
| WWW | www | 80 |
| POP3 | pop3 | 110 |

Table 17-6 Common TCP Port Names and Numbers

Table 17-7 shows some of the common UDP port names and numbers.

| Port Name | Command Parameter | Port Number |
|---|---|---|
| DNS Query | dns | 53 |
| TFTP | tftp | 69 |
| SNMP | snmp | 161 |
| IP RIP | rip | 520 |

Table 17-7 Common UDP Port Names and Numbers

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1014_0.png)

**EXAM TIP** One common problem that occurs when setting up an ACL is that the administrator specifies the wrong protocol for the application, such as TCP for TFTP, RIP, or DNS queries, instead of UDP. This is also true of port numbers or names. You need to be familiar with the TCP/IP protocols and their ports when setting up filtering policies. Remember the TCP and UDP application names and numbers in Tables 17-6 and 17-7 for the exam.

## The established Keyword

The **established** keyword is used only for TCP connections. The assumption behind the use of this keyword is that you are originating TCP traffic on the inside of the network and filtering the returning traffic as it comes back into your network. In this situation, this keyword allows (or denies) any TCP traffic that has a certain flag or flag bits set in the TCP segment header, indicating that this is returning traffic back into your network.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1015_0.png)

**EXAM TIP** For the Cisco CCNA exam, understand the use of the **established** keyword with TCP ACL statements.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1015_1.png)

**VIDEO 17.11.** The digital resources that accompany this book contain a multimedia demonstration of configuring an extended numbered ACL to allow telnet traffic through a router.

## ICMP

The following command shows the syntax of filtering ICMP traffic:

```
Router(config)# access-list {100-199 | 2000-2699} {permit | deny} icmp
    source_address source_wildcard_mask
    destination_address destination_wildcard_mask
    [icmp_message] [log]
```
