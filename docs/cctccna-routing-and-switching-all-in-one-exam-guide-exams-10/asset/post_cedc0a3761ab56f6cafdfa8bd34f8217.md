
- 4. Once flash memory has been initialized, you can copy from the computer (using Xmodem) to flash memory (using the case-sensitive filename of your IOS stored on the computer):

```
switch: copy xmodem: flash:c3750-ipbase-mz.122-35.SE5.bin
```

You will see a prompt stating to begin the Xmodem transfer.

- 5. In Tera Term, choose File | Transfer | Xmodem | Send to send a file via Xmodem.

- 6. When you are prompted for the filename, browse to the folder on your computer and choose the file. The transfer will then begin (see Figure 8-5).

![](img/0eafdcebe219ef3756f0e57bcf1eb997_414_0.png)

Figure 8-5 Using Xmodem to restore an IOS image

- 7. After the transfer has completed you can test the loading of the new IOS by typing:

```
switch: boot flash:c3750-ipbase-mz.122-35.SE5.bin
```

![](img/0eafdcebe219ef3756f0e57bcf1eb997_415_0.png)

TIP The default baud rate within Xmodem is 9600, but you can increase the baud rate within ROMMON or Tera Term before starting the file transfer to speed up the copy operation. The ROMMON command to increase the baud rate is set BAUD 57600.

## Exercise 8-3: Backing Up and Restoring Device Configuration and IOS

In this exercise, you will back up VAN-R1’s device configuration to the TFTP server and then restore it from the TFTP server. You will then back up the IOS to the TFTP server so that it can be restored at a later time.

- 1. Ensure that you performed Exercise 8-2, and have a TFTP server available at the IP address 12.0.0.100.
- 2. To back up the running configuration of VAN-R1 to the TFTP server, enter the following commands:
- 3. Before restoring the configuration, make a change to your device, such as changing the hostname:

```
VAN-R1>enable
VAN-R1#copy running-config tftp
Address or name of remote host []? 12.0.0.100
Destination filename [VAN-R1-config]? VAN-R1-config
Writing running-config....!!
[OK - 882 bytes]
882 bytes copied in 3.015 secs (292 bytes/sec)
```

```
VAN-R1#config term
Enter configuration commands, one per line. End with CNTL/Z.
VAN-R1(config)#hostname Test
Test(config)#
```

4. Now restore your configuration from the TFTP server to the running-config to verify that the old hostname is restored:

```
Test(config)#exit
Test#copy tftp running-config
Address or name of remote host []? 12.0.0.100
Source filename []? VAN-R1-config
Destination filename [running-config]? <<ENTER>>
Accessing tftp://12.0.0.100/VAN-R1-config...
Loading VAN-R1-config from 12.0.0.100: !
[OK - 882 bytes]
882 bytes copied in 0.001 secs (882000 bytes/sec)
VAN-R1#
```

Notice that the hostname of VAN-R1 was restored.

5. Now use the same strategy to back up the Cisco IOS in case it becomes corrupted and needs to be restored. To look at the contents of your flash memory, use the following command:

```
VAN-R1#show flash
```

| File | Length | Name/status |
|------|--------|-------------|
| 3 | 33591768 | c2900-universalk9-mz.SPA.151-4.M4.bin |
| 2 | 28282 | sigdef-category.xml |
| 1 | 227537 | sigdef-default.xml |

[33847587 bytes used, 221896413 available, 255744000 total]
249856K bytes of processor board System flash (Read/Write)

6. To back up the Cisco IOS file to the TFTP server, use the following commands:

```
VAN-R1#copy flash: tftp:
Source filename []? c2900-universalk9-mz.SPA.151-4.M4.bin
Address or name of remote host []? 12.0.0.100
Destination filename [c2900-universalk9-mz.SPA.151-4.M4.bin]? <<ENTER>>
Writing c2900-universalk9-mz.SPA.151-4.M4.bin...!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
[OK - 33591768 bytes]

33591768 bytes copied in 1.061 secs (3324219 bytes/sec)
VAN-R1#
```

If you ever needed to restore the IOS from the TFTP server to flash memory, you could use the **copy tftp: flash:** command and follow the prompts.

## Remote Access to Your Cisco Device

In many instances, it may not be possible to be physically in front of your IOS device to manage it. You can optionally manage it remotely by accessing its CLI via telnet or SSH, or you can manage it with a GUI with a web browser. To access your IOS device’s CLI remotely, you must first set up its VTYs (discussed in Chapter 7). If you’re accessing a layer 2 IOS switch, you’ll need to assign an IP address to a VLAN interface (discussed in Chapter 9); if you’re accessing a router, you’ll need to assign an IP address to one of its interfaces and enable it (discussed in Chapter 7). By default, only telnet is enabled on the router; this section will discuss how you enable SSH.

### SSH vs. Telnet Access

One of the most common tools used by network administrators to manage their devices remotely is the telnet application. Telnet enables you access to the CLI of a device. The problem with telnet, however, is that all information sent between you and the Cisco device is sent in clear text, including your username and/or password. Since you don’t want someone eavesdropping on your connection and seeing everything you do—logging in, viewing the operation of the device, and configuring the device—you need to protect yourself by encrypting the traffic.

The easiest way to accomplish this is to replace the use of telnet with SSH (Secure Shell). SSH uses RSA as an encryption algorithm to encrypt any data sent between you and your networking device. SSH is actually disabled by default on your IOS device.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_418_0.png)

**EXAM TIP** The exam will expect you to know the basics of telnet versus SSH. Telnet sends traffic in clear text, including authentication traffic, making it susceptible to an eavesdropping attack. To secure remote access to your IOS device and prevent eavesdropping on the commands you’re entering and the configurations you’re viewing, use SSH as your remote-access terminal connection. SSH encrypts all traffic, including authentication traffic. To prevent an access attack against your IOS device, you should combine SSH with filtering of management access to your device by using access control lists (which indicate what IP addresses can manage it).

Let’s start by looking at how to configure your device for remote administration via telnet, and then we’ll look at how to configure SSH on your Cisco device to support more secure remote administration.

## Telnet Configuration

In this section, you will learn how to configure telnet on a Cisco device. Then you’ll learn how to use telnet to make a connection to a Cisco device after it has been configured.

### Configuring Telnet

Before you can remote connect to the router using telnet, you need to ensure that an IP address is assigned to the device. On a Cisco router, this will be an IP address on the interface, but for a Cisco switch, you will assign an IP address to the management VLAN (typically VLAN 1). Note that if you are going to telnet into a switch from another network, you will need to set the default gateway setting on the switch so that the switch can send data off the network.

After configuring an IP address, you’ll then configure VTY passwords and a password (or secret value) to enter privileged EXEC mode. On a Cisco device, you cannot telnet into the device if those passwords are not set. Use the following commands to configure a password on VTY ports:

```
VAN-R1>enable
VAN-R1#config term
Enter configuration commands, one per line. End with CNTL/Z.
VAN-R1(config)#line VTY 0 15
VAN-R1(config-line)#password telnetpass
VAN-R1(config-line)#login
```

![](img/0eafdcebe219ef3756f0e57bcf1eb997_419_0.png)

VIDEO 8.05. The digital resources that accompany this book contain a multimedia demonstration of configuring telnet on your Cisco router.

### Using Telnet

If you’ve configured your Cisco devices correctly (with IP addressing and routing information and the appropriate commands on the VTYs), you should be able to telnet to them successfully. However, if you followed the advice mentioned later in the “SSH Configuration” section, you may need to test connectivity with SSH instead. Cisco routers and switches support both incoming and outgoing telnet and SSH. This assumes you have set up the VTYs and configured your IP addressing correctly.
