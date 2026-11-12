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

![](bbox=[0.14, 0.69, 0.24, 0.75])

**EXAM TIP** Remember for the CCNA exam that if you telnet into a Cisco device, and you see the message “Password required but none set,” then you have not configured a password on the line with the **password** command and won’t be able to log in until you configure one. Remember that you should secure your VTYs with a password and login