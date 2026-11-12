
the DTE end of the connection, you do not need to set the clock rate.

12. Save the new changes to NVRAM.

13. To verify that LA-R1 can ping the IP address of VAN-R1, type the following commands:

```
LA-R1#ping 13.0.0.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 13.0.0.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
```

14. On LA-R1, use the following commands to review your configuration (take a few minutes and look at the output of each):

```
LA-R1#show interfaces
LA-R1#show interfaces g0/0
LA-R1#show ip interface brief
```

15. On VAN-R1, use the following commands to review your configuration (take a few minutes and look at the output of each):

```
VAN-R1#show interfaces
VAN-R1#show interfaces g0/0
VAN-R1#show ip interface brief
```

16. Leave the routers up and running for the next exercise.

## Basic IOS Security Configuration

The Computer Security Institute (CSI) is an organization that provides education and training on network security fundamentals. It periodically performs market research studies on security issues with companies. In one study, more than 70 percent of all the companies polled reported experiencing some kind of security breach. Knowing this information, as a network administrator, one of your roles will be to ensure that access to your networking devices is strictly controlled. The following sections will introduce you to securing your IOS device.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_342_0.png)

**NOTE** The information introduced in this chapter includes only a few of the security features you’ll set up on your IOS devices; many more of these features are discussed throughout the book, such as port security on the switches in Chapter 9 and access control lists (ACLs) in Chapter 17, to name a couple.

## Physical and Environmental Threats

Many network administrators forget that the first line of defense in securing their network devices includes protecting against physical and environmental threats, including the following:

- **Hardware** Restricting physical access to the router via physical measures, such as locked doors, key cards, video cameras, and so on
- **Electrical** Ensuring that equipment is protected from power surges/spikes, brownouts, and loss of power
- **Environmental** Protecting equipment from temperatures that are too cold or too hot, or from too much humidity
- **Maintenance** Ensuring that you follow electrostatic discharge procedures when handling sensitive electronic parts, have spares on hand for critical parts, and properly label equipment and cables

An in-depth discussion of these issues is beyond the scope of this book; they are covered in a bit more depth in Cisco’s CCNA and CCNP Security courses.

## Passwords

The most common way of restricting physical access to IOS devices is to use some type of user authentication. For example, you can configure passwords to restrict access to the lines on IOS devices (user EXEC mode) as well as access to privileged EXEC mode. Configuring passwords on Catalyst IOS switches, such as the 2950s and 2960s, is the same as configuring passwords on IOS routers. The following sections discuss the configuration of passwords on IOS devices.

## User EXEC Password Protection

Controlling access to user EXEC mode on an IOS device is accomplished on a line-by-line basis: console, auxiliary, TTYs, and VTYs. Remember that not all devices support auxiliary ports, only routers support TTYs, and the number of VTYs an IOS device supports is product-dependent, ranging from five VTYs (0–4) to almost a thousand.

To secure the console port, you must first go into the console’s line subconfiguration mode with the line console 0 command to configure the line password:

```
IOS(config)# line console 0
IOS(config-line)# password console_password
IOS(config-line)# login
```

The 0 in the first command specifies the console port. Lines and interfaces are numbered from 0 upward. Even though IOS devices have only a single console port, it is designated as 0 (this is true of most, but not all, Cisco products). Next, notice that the prompt on the second line changed. Once you are in line subconfiguration mode, you can use the password command to assign the console password. The final command of login is needed as an indication that you want the Cisco device to force people to authenticate using that password if they connect to the console 0 port. Passwords on IOS devices are case-sensitive. Remember that the password command, when executed under line console 0, sets the user EXEC password for someone trying to access the IOS device from the console port only.

The auxiliary port is typically used as a remote access port with a modem attached to it, which enables you to dial into the router if something has happened to the Internet connection and you cannot remote in via telnet or SSH. The following code shows the syntax for setting up password authentication on the auxiliary port:

```
Router(config)# line aux 0
Router(config-line)# password aux_password
Router(config-line)# login
```

Again, the **login** command is required; if you forget it, you will have set a password on the port and will not have asked the Cisco device to prompt for that password when someone connects to the port.

To set up a telnet password for your VTYs, use this configuration:

```
IOS(config)# line vty 0 15
IOS(config-line)# password telnet_password
IOS(config-line)# login
```

The **vty** parameter in this command refers to *virtual terminal*, a fancy name for telnet or SSH access. Cisco devices support multiple simultaneous VTY (telnet) sessions, where each connection is internally tracked by a number: 0–15. Depending on the router model and IOS software version, this number may range from 5 (0–4) on up to almost 1000. You could assign a different password to each VTY, but then you wouldn’t know which password to use when telnetting into the IOS device. However, IOS enables you to specify multiple VTYs ports with the **line** command, simplifying your configuration, as in the preceding configuration: **line vty 0 15**. You need to specify the beginning and ending VTY numbers on the same line.

Once you are in line subconfiguration mode, use the **password** command to set your password. You also need to enter the **login** command to force authentication on those ports, essentially telling the device to prompt for the password if someone tries to connect via telnet.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_344_0.png)

**EXAM TIP** Remember for the CCNA exam that if you telnet into a Cisco device, and you see the message “Password required but none set,” then you have not configured a password on the line with the **password** command and won’t be able to log in until you configure one. Remember that you should secure your VTYs with a password and login process. Another security option to secure your VTY ports is to apply an ACL. ACLs are discussed in Chapter 17.

If you specify only one number when configuring your VTYs, such as the following, then you are configuring only that VTY:

```
IOS(config)# line vty 0
IOS(config-line)# password cisco123
IOS(config-line)# login
```

In this example, only the VTY 0 line is assigned a password, and authentication is enforced by requiring someone to log into the port. If this was the only item you configured for your VTYs on a factory-default IOS device, then only one person could remotely access the IOS device using telnet or SSH via VTY 0.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_345_0.png)

VIDEO 7.06. The digital resources that accompany this book contain a multimedia demonstration of configuring passwords on your line ports.

## Creating Usernames and Passwords

Other ways of validating access are available, such as a local username database or an external authentication server. A local username database on the router enables you to specify both a username and a password to restrict access to the lines on an IOS device. Using usernames and passwords is recommended over using only passwords on lines: the advantage of this approach is that each user can have his or her own password instead of sharing a password, providing for more accountability. The following commands illustrate the setup of a local username database and its use on VTYs:

```
IOS(config)# username name {secret | password} password
IOS(config)# line vty 0 15
IOS(config-line)# login local
```
