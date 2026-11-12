
**NOTE** Here I am demonstrating how to telnet into a Cisco device from another Cisco device, but you can also telnet into a Cisco device from a laptop or desktop computer if you have a telnet client running on the system. Examples of telnet client software are PuTTY and Tera Term.

To open up a telnet session from your IOS device, you can use any of the following three methods:

```
IOS# name_of_the_destination | destination_IP_address
```

or

```
IOS# telnet name_of_the_destination | destination_IP_address
```

or

```
IOS# connect name_of_the_destination | destination_IP_address
```

All three of these methods work in the same manner: they all have IOS attempt to telnet to the specified destination.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_420_0.png)

![](img/0eafdcebe219ef3756f0e57bcf1eb997_420_1.png)

**TIP** If you mistype a command name from the CLI, the IOS assumes you’re trying to telnet into a system with that name and attempts to resolve the name to an IP address using the local host table, or a DNS server query. This freezes up the CLI until the query attempt times out. To stop this behavior, use the **no ip domain-lookup** command.

## Suspending Telnet Sessions

If you are using an IOS device and telnet to a remote destination, you may want to go back to your IOS device. One way of doing this is to exit the remote device; however, you may just want to go back to your source Cisco device, make a quick adjustment, and then return to the remote device. Logging off and back on to the remote device is a hassle in this instance.

Cisco, however, has solved this problem by allowing you to *suspend* a telnet session, return to your original router or switch, do what you need to do, and then jump right back into your remote device—all without your having to log off and back on to the remote device. To suspend a telnet session, press CTRL-SHIFT-6, X (hold down the CTRL, SHIFT, and 6 keys simultaneously, let go, and then press the X key).

On your source IOS, if you want to see the open telnet sessions that are currently suspended, use the **show sessions** command:

```
IOS# show sessions
Conn Host Address Byte Idle Conn Name
  1 10.1.1.1 10.1.1.1 0 1 10.1.1.1
* 2 10.1.1.2 10.1.1.2 0 2 10.1.1.2
```

This example shows two open telnet sessions. The one with the asterisk (*) preceding it is the default (last accessed) session. To resume the last session, all you have to do is press ENTER on an empty command line.

To resume a specific session, use this command:

```
Router# resume connection_#
```

The connection number to enter is the number in the Conn column of the **show sessions** command. As a shortcut, you can just list the connection number without including **resume**; this will accomplish the same thing. If you are on the source router or switch and want to terminate a suspended telnet session without having to resume and then log out of the telnet session, you can use this command:

```
Router# disconnect connection_#
```

## Verifying and Clearing Connections

If you are logged into an IOS device, you can view other users who are also logged in with this command:

```
Router# show users
      Line        User     Host(s)    Idle     Location
        0     con 0                 idle
        2     vty 0     idle          0     10.1.1.1
*       3     vty 1     idle          0     10.1.1.2
```

The asterisk in the first column indicates the current session.

If you want to terminate someone’s session, use the privileged EXEC clear line command:

```
Router# clear line line_#
```

The line number that you enter here should match the appropriate number in the Line column of the output of the show users command.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_422_0.png)

**EXAM TIP** For the exam, you’ll need to remember how to suspend, resume, and disconnect telnet sessions. Use the CTRL-SHIFT-6, x control sequence to suspend a telnet session. Pressing ENTER on a blank command line resumes the last suspended telnet session. Use the **resume** command to resume a suspended telnet connection. Use the **show sessions** command to see your suspended telnet sessions. Use the **disconnect** command to disconnect a suspended telnet session.

## Exercise 8-4: Configuring Telnet on the Cisco Device

In this exercise you will configure the VAN-R1 router so that you can telnet into it and perform remote administration. This exercise assumes the same network configuration as the previous exercises in this chapter.

1. Before you can administer the Cisco device remotely, passwords must be configured for privileged EXEC mode:
   ```
   VAN-R1>enable
   VAN-R1#config term
   Enter configuration commands, one per line. End with CNTL/Z.
   VAN-R1(config)#enable password enablepass
   VAN-R1(config)#enable secret secretpass
   ```

2. Next, configure passwords on the VTY lines (telnet ports):
   ```
   VAN-R1(config)#line vty 0 15
   VAN-R1(config-line)#password telnetpass
   VAN-R1(config-line)#login
   ```

3. Console into router LA-R1 and then enter **ping 12.0.0.1**. You should receive replies of exclamation marks (!), which verifies that you can communicate with VAN-R1. Note that your prompt is LA-R1 because that is the router you are connected to right now.

4. To telnet into VAN-R1, enter **telnet 12.0.0.1**. You should receive a prompt asking for the password. Keep in mind that this is the password for the VTY ports.

5. Enter **telnetpass** as the password and press ENTER.

6. You should now have a connection to VAN-R1 (notice that your prompt has changed). You can now enter any commands you want to administer the VAN-R1 device.

7. To disconnect temporarily from the session and go back to LA-R1, press CTRL-SHIFT-6, X. You should see that you are now on LA-R1 and can administer LA-R1.

8. If you want to see the telnet sessions that are open, use the **show sessions** command:
   ```
   LA-R1>show sessions
   Conn Host              Address      Byte Idle Conn Name
   * 1 12.0.0.1            12.0.0.1      0    2   12.0.0.1
   ```

9. In this example, there is only one session with the ID of 1, connected to 12.0.0.1. To connect to that session, use the **resume 1** command and press ENTER twice. You will return to the VAN-R1 device.

10. To disconnect permanently from the telnet session, type **exit** and press ENTER.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_424_0.png)

**VIDEO 8.05.** The digital resources that accompany this book contain a multimedia demonstration of using the telnet command to remotely connect from one device to another.

## SSH Configuration

To set up SSH on your Cisco device so that you can use an SSH client to access it, you’ll need to configure the following:

- **A local username and password**: SSH requires both a username and password configured on the device (the **username** command configures both).
- **A hostname and a domain name**: Hostname and domain name are required to label the RSA key pair on the IOS device (**hostname** and **ip domain-name** commands).
- **The SSH version to use**: You should configure the specific SSH version you want to use. The default is version 1, but the recommended version to use is 2 (**ip ssh version** command).
- **RSA public and private keys**: You will need to generate the encryption keys. These are used to encrypt and decrypt data that travels through the remote-access connection (**crypto key generate rsa** command).
- **Restricting VTY access**: Finally, you will ensure that remote access can be achieved only through SSH and not telnet, because telnet does not encrypt communication. By default, telnet is allowed on the VTYs—you should ensure that only SSH access is allowed (**login local** and **transport input** line subconfiguration commands).

Here is an example configuration for setting up SSH:

```
IOS(config)# username glen secret mypassword
IOS(config)# hostname R1
R1(config)# ip domain-name gleneclarke.com
R1(config)# ip ssh version 2
Please create RSA keys to enable SSH. R1(config)# crypto key generate rsa
The name for the keys will be: R1.gleneclarke.com
Choose the size of the key modulus in the range of 360 to 2048 for
your General Purpose Keys. Choosing a key modulus greater than 512
may take a few minutes. How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
*Oct  5 16:48:23.455: %SSH-5-ENABLED: SSH 2.0 has been enabled
R1(config)# line vty 0 15
R1(config-line)# login local
R1(config-line)# transport input ssh
R1(config-line)# exit
```

![](img/0eafdcebe219ef3756f0e57bcf1eb997_425_0.png)

NOTE The **transport input** command on a line restricts remote-access connectivity to the device by the protocol you specify.

Notice in this example that when you execute the **crypto key generate rsa** command, you are prompted for the length of the RSA keys. The longer the keys, the more secure your connection will be, with 2048-bit keys being the strongest. Choosing a higher value, however, will take longer for the IOS device to generate. On IOS devices shipped today, this shouldn’t take that long: it took me about 30 seconds to generate 2048-bit keys on an 1841 router.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_425_1.png)
