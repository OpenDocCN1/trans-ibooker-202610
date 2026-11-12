# CDP Configuration

As mentioned in the last section, CDP is enabled on all Cisco CDP–capable devices when you receive your product from Cisco. On Cisco routers and switches, you can globally disable or enable CDP with this command:

```
IOS(config)# [no] cdp run
```

You can also enable or disable CDP on an interface-by-interface basis:

```
IOS(config)# interface type [slot_#/]port_#
IOS(config-if)# [no] cdp enable
```

Since CDP doesn’t use many IOS resources (a small frame is generated once a minute), it is recommended that you keep it enabled unless your router is connected to the Internet or untrusted devices; then you should at least disable CDP on these interfaces. At a minimum, the information is only 80 bytes in length. Other, optional commands are related to CDP, such as changing the update and hold-down timers, but these commands are beyond the scope of this book.

![](bbox=[0.14, 0.53, 0.24, 0.595])

**EXAM TIP** For the Cisco CCNA exam, you should know how to disable CDP globally on the device with the **no cdp run** command, and on a specific interface with the **no cdp enable** command.

# CDP Status

To see the status of CDP on your Cisco device, use this command:

```
IOS# show cdp
Global CDP information:
Sending CDP packets every 60 seconds
Sending a holdtime value of 180 seconds
Sending CDPv2 advertisements is  enabled
```