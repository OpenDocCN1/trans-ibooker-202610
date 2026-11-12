know and type in the MAC address, you can use the *sticky option*, which means that the switch will dynamically configure port security with whatever connects to the port. If you set the maximum number of addresses to learn to *1*, then **sticky** will learn only the address of the currently connected device and will configure port security with that MAC address:

```
VAN-SW1(config-if)#switchport port-security
VAN-SW1(config-if)#switchport port-security mac-address sticky
VAN-SW1(config-if)#switchport port-security maximum 1
```

![](bbox=[0.14, 0.3, 0.24, 0.37])

**EXAM TIP** For the CCNA exam, remember that the **sticky** option with port security is used to restrict the port to the system that is currently connected to that port. It saves the administrator from having to learn the MAC address of that client and then inputting it into the port security command.

Lastly, specify what happens when an unauthorized device connects to the port on the switch with the violation mode. Look back to Chapter 9 for a review of the violation modes; here we’ll disable the port by using the shutdown mode:

```
VAN-SW1(config-if)#switchport port-security violation shutdown
```

![](bbox=[0.14, 0.67, 0.22, 0.74])

**VIDEO** 17.08. The digital resources that accompany this book contain a multimedia demonstration of configuring port security on the switch.

# DHCP Snooping