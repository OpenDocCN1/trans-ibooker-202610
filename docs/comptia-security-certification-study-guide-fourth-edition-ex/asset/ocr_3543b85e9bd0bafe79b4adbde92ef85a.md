erasable programmable read-only memory. You can normally go to the manufacturer's web site for the device and download a revised, updated version of the firmware. This version of the firmware will contain fixes to any bugs at the time it was created.

After you update the firmware, be sure you have configured a password for the device so that only authorized individuals can manage it. Most devices today, such as routers and switches, support a web management interface, which you should password-protect and configure to use HTTPS if supported.

When looking at hardening the network, ensure you are using the most secure devices possible. For example, use switches instead of hubs because switches have a filtering feature that ensures the switch sends the traffic only to the port on which the destination system resides. This means that it is hard for someone to put a sniffer on the network and capture the traffic because the traffic never reaches the device doing the packet sniffing. Back when hubs were popular network devices, the packets would be sent to all ports on the hub, and anyone with a sniffer installed on a system could view all the network traffic.

# Port Security

Most network switches today support a feature known as port security that you can use to limit which systems can connect to a port on the switch by listing specific MAC addresses with the port. Port security is also known as MAC limiting because you are limiting by MAC addresses which systems can connect to a port.

The following code shows how you can configure port security on a Cisco switch. This example limits which systems can connect to port 1 on the switch by configuring a MAC address for that switch port:

```
NY-SW1>enable
NY-SW1#config term
NY-SW1(config)#interface f0/1
NY-SW1(config-if)#switchport mode access
NY-SW1(config-if)#switchport port-security
NY-SW1(config-if)#switchport port-security mac-address 0000.1111.2222
NY-SW1(config-if)#switchport port-security maximum 1
NY-SW1(config-if)#switchport port-security violation shutdown
```

Let’s review the preceding code. The first three lines are used to navigate to Fast Ethernet port 1 on the switch. Then the port is placed in access mode so that the workstation can connect to the port, and the port security feature is enabled with the switchport port-security command. The lines that follow are used to configure a MAC address (0000.1111.2222) for the port and then to configure a maximum of one address for that port so that the switch does not learn any additional MAC addresses for it. Finally, you configure the port to disable (shutdown) if there is an address violation, meaning if someone with a different MAC address connects to the port, you want to disable the port so that network access is not allowed.