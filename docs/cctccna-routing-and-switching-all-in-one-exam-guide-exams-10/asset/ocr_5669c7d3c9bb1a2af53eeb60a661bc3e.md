after it has been configured.

# Configuring Telnet

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

![](bbox=[0.14, 0.57, 0.22, 0.63])

VIDEO 8.05. The digital resources that accompany this book contain a multimedia demonstration of configuring telnet on your Cisco router.

# Using Telnet

If you’ve configured your Cisco devices correctly (with IP addressing and routing information and the appropriate commands on the VTYs), you should be able to telnet to them successfully. However, if you followed the advice mentioned later in the “SSH Configuration” section, you may need to test connectivity with SSH instead. Cisco routers and switches support both incoming and outgoing telnet and SSH. This assumes you have set up the VTYs and configured your IP addressing correctly.