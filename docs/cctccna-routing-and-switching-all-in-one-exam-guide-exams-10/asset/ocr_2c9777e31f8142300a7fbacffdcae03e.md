the prompt changes, signifying that you are in interface subconfiguration mode for the specified interface—**(config-if)#** in this example.

![](bbox=[0.14, 0.17, 0.24, 0.23])

**TIP** You can’t determine what interface you are working in by examining the prompt. If you aren’t sure, use the command recall feature to recall the **interface** command and re-execute it.

When specifying the interface, you can use any of the following nomenclatures:

```
IOS(config)# interface GigabitEthernet 0/1
IOS(config)# interface GigabitEthernet0/1
IOS(config)# int g 0/1
IOS(config)# int g0/1
IOS(config)# int gig 0/1
IOS(config)# int gig0/1
```

![](bbox=[0.14, 0.55, 0.24, 0.64])

**NOTE** You can separate the type and location with a space, or concatenate the two together. Likewise, you can abbreviate the commands and parameters.

# Assign IP Address

After navigating to the interface, you can execute a number of commands to modify the characteristics of the interface. On a Cisco router, for example, you may want to assign an IP address to an interface with the following commands: