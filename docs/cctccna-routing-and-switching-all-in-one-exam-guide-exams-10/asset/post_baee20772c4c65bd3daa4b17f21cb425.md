
6. The **show controller** command shows whether an interface is DTE/DCE; **shutdown** disables an interface; **login local** specifies the use of an IOS database for authentication; **clock rate** specifies the speed of a serial interface. The following shows the correct command with its definition:

| Command | Description |
|---|---|
| A. shutdown | B Shows if an interface is DTE/DCE |
| B. show controller | C Specifies the use of an IOS database for authentication |
| C. login local | D Specifies the speed of a serial interface |
| D. clock rate | A Disables an interface |

7. **B.** When you execute the **line console 0** command, your prompt changes to **Router(config-line)#**.

8. The **service password-encryption** command will encrypt unencrypted passwords.

9. **C.** The **banner exec** command displays a banner after the user authenticates, but before the EXEC prompt is displayed.

10. **B.** The “...line protocol is down” message refers to a problem in the data link layer.

11. **D.** Use the **copy running-config startup-config** command to back up your configuration from RAM to NVRAM.

## Performance-based Answers

The following are the answers to the performance-based questions for this chapter. Note that on the real Cisco exams it is common to drag the items from the left side of the screen to the matching items on the right side of the screen.

1. The following shows the matching commands for each description.

### Description
Manually assigns an IP address to an interface

Configures the interface to receive an IP address from the DHCP server

Configured on the DCE end of a point-to-point serial link

Restricts the VTY ports to accept only SSH traffic

Configures a DHCP server

Configures the console to timeout after 5 minutes of inactivity

### Command
```
Router(config-line)# exec-timeout 5.0
```
```
Router(config-line)# transport input ssh
```
```
Router(config-if)#ip address 14.0.0.1 255.0.0.0
```
```
Router(config-if)#ip address dhcp
```
```
Router(config-if)#clock rate 64000
```

2. The following shows the command associated with each of the boxes showing command output.

```
VAN-R1# show controllers s0/3/0
Interface Serial0/3/0
Hardware is PowerQUICC MPC860
DCE V.35, clock rate 64000
idb at 0x81081AC4, driver data structure at 0x81084AC0
SCC Registers:
General [GSMR]=0x02:0x00000000, Protocol-specific [PSMR]=0x8
Events [SCCE]=0x0000, Mask [SCOM]=0x0000. Status [SCS]=0x00
Transmit on Demand [TODR]=0x0, Data Sync [DSR]=0x7E7E
```
```
VAN-R1# show running-config
Building configuration...
Current configuration : 959 bytes
!
version 15.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname VAN-R1
!
```
```
VAN-R1# show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     12.0.0.1        YES manual   up                    up
Serial0/3/0            13.0.0.1        YES manual   up                    up
Serial0/3/1            unassigned      YES unset    administratively down down
Vlan1                  unassigned      YES unset    administratively down down
```
```
VAN-R1# show interface g0/0
GigabitEthernet0/0 is up, line protocol is up (connected)
Hardware is CN Gigabit Ethernet, address is 0009.7c04.ab01 (bia 0009.7c04.ab01)
Internet address is 12.0.0.1/8
MTU 1500 bytes, BW 1000000 Kbit, DLY 10 usec,
reliability 255/255, txload 1/255, rxload 1/255
Encapsulation ARPA, loopback not set
Keepalive set (10 sec)
Full-duplex, 100Mb/s, media type is RJ45
output flow-control is unsupported, input flow-control is unsupported
```

## Cisco Device Management

In this chapter, you will
- Learn about router configuration files
- Learn how to make changes in your network
- Get familiar with IOS image files
- Learn how to gain remote access to your Cisco device
- Learn how to perform configuration register changes and password recovery
- Learn how to troubleshoot and debug
- Learn about licensing management

This chapter covers important IOS features that you can use to manage your IOS device. Many of these features are supported across all IOS devices, but some are supported on only certain devices. This chapter offers an in-depth discussion of configuration files. It also discusses how to upgrade your IOS device and remotely access it via Secure Shell (SSH). You can use many tools on your IOS device for troubleshooting connection problems, including the Cisco Discovery Protocol (CDP), ping, traceroute, telnet, and debug. These tools are discussed toward the end of the chapter.

### Router Configuration Files

You had a basic introduction to configuration files in Chapter 7. Remember that a configuration file contains the commands used to configure an IOS device. Configuration files are typically located in one of three places: RAM, NVRAM, and/or an external server, such as a TFTP, FTP, HTTP, or Secure Copy Protocol (SCP) server. The configuration that the router is
currently using is in RAM. You can back up or save this configuration either to NVRAM or to an external server.

As you may recall from Chapter 7, the commands *related* to configuration files, even **show** commands, require you to be working in privileged EXEC mode. Also, the running configuration of an IOS device is not automatically saved to NVRAM—you must manually do this by issuing the **copy running-config startup-config** command. The following sections show you how to manipulate your configuration files.

### Saving Configuration Files

Chapter 7 explained how to save your configuration from RAM to NVRAM with the **copy running-config startup-config** command. When you execute this command, whatever filename (the default is *startup-config*) you are copying to in NVRAM is completely overwritten. If you want to keep an old copy *and* a newer one in NVRAM, you’ll need to specify a name other than startup-config. Note that the **copy** command has two parameters: the first parameter refers to where the source information is (where you want to copy from) and the second parameter refers to where the destination is (where you want to copy to).

You can copy your running-config or startup-config configuration file to flash, like this:

```
IOS# copy running-config flash:file_name
IOS# copy startup-config flash:file_name
```

This enables you to have multiple configuration files stored locally on your IOS device; by default, when booting up, your IOS device will use the startup-config file in NVRAM to load its configuration.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_383_0.png)

**TIP** It is not a common practice to copy configuration files to flash, and for exam purposes, this is not where you back up configuration files.

However, I commonly do this when an FTP or TFTP server currently isn’t available.

You can also back up your configuration to an external server. You must have the server software on a server or PC and IP configured correctly on your IOS device to access the server. The syntax looks like this on your IOS device:

```
IOS# copy running-config URL_location
```

So, for example, to back up your configuration file to a TFTP server, the configuration would look like this:

```
IOS# copy running-config tftp://192.168.1.10/mybackupfile.txt
```

The configuration is backed up to an ASCII text file. If you don’t supply the full URL, just the protocol information, you’ll be prompted for the additional information:

```
IOS# copy running-config tftp
Address or name of remote host []? 192.168.1.10
Destination filename [router-config]? mybackupfile.cfg
!!
781 bytes copied in 5.8 secs (156 bytes/sec)
IOS#
```

If the filename already exists on the server, the server overwrites the old file. After entering this information, you should see bang symbols (!) indicating the successful transfer of UDP segments to the TFTP server. If you see periods (.), this indicates an unsuccessful transfer. Plus, upon a successful transfer, you should also see how many bytes were copied to the server.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_384_0.png)
