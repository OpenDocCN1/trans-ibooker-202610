
authorized device. Port security associates the MAC address with the port of the authorized system.

- To prevent rogue DHCP servers from sending out DHCPOFFER messages on the network, use DHCP snooping.
- To prevent ARP poisoning attacks on the network, use dynamic ARP inspection, which validates the IP address and MAC address in an ARP message against the DHCP snooping binding database.

## Implementing Access Control Lists

- Standard ACLs are used to filter traffic by the source IP address only and have an ACL number assigned to them that ranges from 1 to 99 or 1300 to 1999.
- Extended ACLs are used to filter traffic based on the source and destination IP addresses, source and destination ports, and protocol information as well. Extended ACLs have an ACL number ranging from 100 to 199 or 2000 to 2699.
- Each ACL has an implicit deny at the end of the ACL, so if a rule does not match the packet being evaluated, the packet is dropped.
- You can create an access list with the **access-list** command (for example, **access-list 1 permit 192.168.2.0 0.0.0.255**).
- After creating the access lists, you apply it to the interface by navigating to the interface and then using the **ip access-group** command (for example, **ip access-group 1 in**).
- You can apply an access list to the VTY ports with the **access-class** command.

## Understanding ACL Placement

- Standard ACLs should be placed as close to the destination devices as possible.
- Extended ACLs should be placed as close to the source devices as possible.

## Questions

The following questions will help you measure your understanding of the material presented in this chapter. Read all the choices carefully because there may be more than one correct answer. Choose the correct answer(s) for each question.

1. What command would you use on a Cisco device to ensure that passwords are encrypted within the configuration files?

- A. password-encryption
- B. service password-encryption
- C. password encrypt on
- D. enable secret

2. You have configured an enable password and an enable secret on your Cisco device. Which one will you need to use when you navigate to privileged EXEC mode?

- A. The enable password
- B. The strongest password of the two
- C. The enable secret
- D. The one that was configured first

3. Looking at the following configuration performed by one of your network administrators, why are admins not prompted for a password when connecting to the console port?

```
VAN-R1>enable
VAN-R1#config term

VAN-R1(config)#line con 0
VAN-R1(config-line)#password conpass
```

- A. Need to configure an enable password
- B. Need to configure an enable secret
- C. Must add the authenticate command
- D. Missing the login command

4. You want to create an access list that filters traffic by the source IP address of the packet only. What type of access list would you use?

- A. Extended
- B. Standard
- C. Extension
- D. Basic

5. Your manager is concerned with rogue DHCP servers being added to the network. What switch feature would you use to prevent rogue DHCP servers?

- A. DHCP snooping
- B. Dynamic ARP inspection
- C. Access control list
- D. Port security

6. You are configuring SSH on a Cisco switch. What two conditions must exist for your configuration to be successful?

- A. The switch must be running a crypto-supported IOS image.
- B. You must have configured a console password.
- C. Routing must be disabled.
- D. You must have run the ip domain-name command.
- E. You must have OSPFv2 running on the switch.

7. You want to configure an access list that ensures only HTTP traffic is allowed to reach the system with the IP address of 10.0.45.10/24. What type of access list would you use?

- A. Extended
- B. Standard
- C. Extension
- D. Basic

8. You have created an access list numbered 100 that you would like to apply to the GigabitEthernet 0/0 interface for inbound traffic. What commands would you use?

- A. Use the following:
```
Router(config)# interface GigabitEthernet 0/0
Router(config-if)# ip access-group 100 ingress
```
- B. Use the following:
```
Router(config)# interface GigabitEthernet 0/0
Router(config-if)# ip access-group 100 egress
```
- C. Use the following:
```
Router(config)# interface GigabitEthernet 0/0
Router(config-if)# access-class 100 in
```
- D. Use the following:
```
Router(config)# interface GigabitEthernet 0/0
Router(config-if)# ip access-group 100 in
```

Use the following configuration to answer questions 9 and 10.

```
VAN-R1>enable
VAN-R1#config term
VAN-R1(config)#enable password ciscopass
VAN-R1(config)#enable secret ciscosecret

VAN-R1(config)#username edtetz password edpass
VAN-R1(config)#username glenclarke password glenpass

VAN-R1(config)#line con 0
VAN-R1(config-line)#password conpass

VAN-R1(config)#line vty 0 15
VAN-R1(config-line)#password telnetpass
VAN-R1(config-line)#login
```

9. In the configuration, administrators are not prompted for usernames and passwords when they try to administer the Cisco device remotely. What can you do to remedy this?

- A. Add the login command to con 0.
- B. Use stronger passwords on the user accounts.
- C. Change the login command to the login local command.
- D. Add the service password-encryption command.

10. What password will administrators need to use to enter into privileged EXEC mode?

- A. glenpass
- B. ciscosecret
- C. conpass
- D. ciscopass

## Performance-based Questions

The Cisco exams have performance-based questions where you must drag an item from the left side of the screen to its proper place on the right side of the screen to answer the question. Following are some sample performance-based questions related to security.

1. You have configured an extended access list with the following rules:

```
access-list 120 deny tcp 145.13.45.100 0.0.7.255 any eq 23
access-list 120 deny tcp 145.13.45.100 0.0.7.255 any eq http
access-list 120 permit ip any any
```

Using the following exhibit, identify which packets would be denied and which packets would be permitted by writing the packet information in the boxes on the right. On the real exam, you would need to drag the box representing the packet to the appropriate category on the right.

Source IP: 145.13.15.29
Destination Port: 23

Source IP: 145.13.42.110
Destination Port: 23

Source IP: 145.13.45.10
Destination Port: 21

Source IP: 145.13.47.15
Destination Port: 80

Source IP: 145.13.51.12
Destination Port: 80

Denied

Permitted

2. Draw a line from the command on the left side to the appropriate description on the right side. Not all descriptions are used.

### Command

- service password-encryption
- switchport port-security violation shutdown
- ip arp inspection vlan 1
- transport input ssh
- ip dhcp snooping trust

### Description

- Restrict VTY access to the SSH protocol
- Deny the usage of SSH
- Encrypt passwords within the configuration file
- Disable the port when a security violation occurs
- Configured on the port your DHCP server is connected to
- Configured on the port that DHCP clients are connected to
- Designed to prevent MiTM attacks

## Answers

- 1. **B.** The **service password-encryption** command is used to encrypt all plain text passwords in the configuration file. It is highly recommended that you do this to prevent someone from viewing your passwords when looking at the configuration on the Cisco device.
- 2. **C.** Both the **enable password** and **enable secret** commands are used to configure a password that is required by administrators if they enter privileged EXEC mode. It is recommended, however, that you use the enable secret, because it is encrypted within the configuration file. If both are set, the secret is the password that must be used by administrators, because it takes precedence.
- 3. **D.** After configuring a password on any of the connection lines such as the console port, auxiliary port, or VTY ports, you must use the **login** command to tell the Cisco device you want authentication on the port.
- 4. **B.** A standard access list is used to filter packets based on the source IP address only.
- 5. **A.** You can prevent rogue DHCP servers from being added to the network by using DHCP snooping features on the switch. When
