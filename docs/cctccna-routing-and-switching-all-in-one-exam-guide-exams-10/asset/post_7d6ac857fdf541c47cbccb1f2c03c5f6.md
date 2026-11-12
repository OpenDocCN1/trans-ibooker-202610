
```
Switch#show port-security
```

| Secure Port | MaxSecureAddr (Count) | CurrentAddr (Count) | SecurityViolation (Count) | Security Action |
|---|---|---|---|---|
| Gig1/0/4 | 1 | 1 | 0 | Shutdown |

```
Switch#
```

To see the MAC addresses that are statically defined or dynamically learned with port security, use the **show port-security address** command:

```
Switch#show port-security address
                Secure Mac Address Table
```

| Vlan | Mac Address | Type | Ports | Remaining Age (mins) |
|---|---|---|---|---|
| 1 | CCCC.CCCC.3333 | SecureConfigured | GigabitEthernet1/0/4 | - |

```
Total Addresses in System (excluding one mac per port)     : 0
Max Addresses limit in System (excluding one mac per port) : 1024
Switch#
```

In this example, notice that port G1/0/4 has a MAC address associated with it.

If you want to see a bit more port security detail on a specific port, use the **show port-security interface** command as follows:

```
Switch#show port-security interface g1/0/4
Port Security               : Enabled
Port Status                 : Secure-down
Violation Mode              : Shutdown
Aging Time                  : 0 mins
Aging Type                  : Absolute
SecureStatic Address Aging  : Disabled
Maximum MAC Addresses       : 1
Total MAC Addresses         : 1
Configured MAC Addresses    : 1
Sticky MAC Addresses        : 0
Last Source Address:Vlan    : 0000.0000.0000:0
Security Violation Count    : 0
```

In this example, you can see that port security is enabled, the violation mode is shutdown, the maximum number of MAC addresses that can be connected to the port is 1, and one MAC address has to be statically configured for the port. At the bottom of the output, notice that no security violations have occurred on the port.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_509_0.png)

**EXAM TIP** Be familiar with the **show port-security interface** command for the exam. Remember that if the Port Security value is set to **Disabled**, you must execute the **switchport port-security** command on the interface to enable it. Also, you’ll be expected to be able to determine the number of MAC addresses configured (allowed).

## Exercise 9-5: Configuring Port Security on a Cisco Switch

In this exercise, you will practice configuring port security on a Cisco switch. Then you’ll verify that port security is working.

- 1. Ensure that all routers and switches are running.
- 2. On ComputerA, launch a command prompt.
- 3. To verify that ComputerA can communicate on the network, type **ping 12.0.0.1** and press ENTER.
  - You should receive four ping replies. This proves that ComputerA can communicate on the network.
  - You will next configure port security with a bogus MAC address on port 5 of the switch, which is not the MAC address of ComputerA. This will cause port 5 to shut down and no longer allow ComputerA to communicate on the network.
- 4. Connect to the console port of VAN-SW1 with PuTTY.
- 5. Navigate to port 5 on the switch:

  ```
  VAN-SW1>enable
  VAN-SW1#config term
  Enter configuration commands, one per line. End with CNTL/Z.
  VAN-SW1(config)#interface g1/0/5
  VAN-SW1(config-if)#
  ```

- 6. To place the port in access mode and then enable the port security feature on port 5, use the following commands:

  ```
  switch(config-if)# switchport mode access
  switch(config-if)# switchport port-security
  ```

- 7. To specify that a maximum of 1 MAC address can be associated with the port, use this command:

  ```
  switch(config-if)# switchport port-security maximum 1
  ```

- 8. To shut down the port until an administrator enables it if an unauthorized device tries to use the port, enter the following:

  ```
  switch(config-if)# switchport port-security violation shutdown
  ```

- 9. To specify the MAC address of the device that is authorized to connect to the port, type the following:

  ```
  switch(config-if)# switchport port-security mac-address cccc.cccc.3333
  ```

- 10. Switch back to the command prompt on ComputerA and try to ping 12.0.0.1.

  Notice that you can no longer ping. This is because the port has been configured to allow only the device with the MAC address of cccc.cccc.3333 to use the port. The port should now be disabled and cannot be used until the administrator enables it.

- 11. Change back to the VAN-SW1 switch and type the following command:

  ```
  VAN-SW1#show port-security
  ```

  Notice that port 5 has one security violation and that the security action was shut down.

  | Secure Port | MaxSecureAddr (Count) | CurrentAddr (Count) | SecurityViolation (Count) | Security Action |
  |---|---|---|---|---|
  | Gig1/0/5 | 1 | 1 | 1 | Shutdown |

- 12. To view the port security details on the port, type the following:

  ```
  VAN-SW1#show port-security interface g1/0/5
  Port Security              : Enabled
  Port Status                : Secure-shutdown
  Violation Mode             : Shutdown
  Aging Time                 : 0 mins
  Aging Type                 : Absolute
  SecureStatic Address Aging : Disabled
  Maximum MAC Addresses      : 1
  Total MAC Addresses        : 1
  Configured MAC Addresses   : 1
  Sticky MAC Addresses       : 0
  Last Source Address:Vlan   : 0030.F2E8.4203:1
  Security Violation Count   : 1
  ```

  Notice that the port status is Secure-shutdown, meaning that the port is shut down because of a security violation.

- 13. Use the show interfaces g1/0/5 command to view the interface stats on the port. Note the err-disabled on the first line, stating that this port was disabled due to error.
- 14. To enable the port once again, use the following commands:

  ```
  VAN-SW1#config term
  VAN-SW1(config)#interface g1/0/5
  VAN-SW1(config-if)#shutdown
  %LINK-5-CHANGED: Interface GigabitEthernet1/0/5, changed state to administratively down
  VAN-SW1(config-if)#no shutdown
  %LINK-5-CHANGED: Interface GigabitEthernet1/0/5, changed state to up
  ```

- 15. Use the show interface g1/0/5 command to see that the interface is up. Note that you cannot use the port from ComputerA, because the port security is still configured, so the port will get disabled again.

## Working with VLANs

A virtual LAN (VLAN) is a logical grouping of network devices in the same broadcast domain that can span across multiple switches. The top part of Figure 9-5 shows an example of a simple VLAN, where every device is in both the same collision and broadcast domains. In this example, a hub is providing the connectivity, which represents, to the devices connected to it, that the segment is a logical segment.

The bottom part of Figure 9-5 shows an example of a switch with four PCs connected to it. One major difference between the switch and the hub is that all devices connected to the hub are in the same collision domain, whereas in the switch example, each port of the switch is a separate collision domain. By default, all ports on a switch are in the same broadcast domain. In this example, however, the configuration of the switch places PC-E and PC-F in one broadcast domain (VLAN) and PC-G and PC-H in another broadcast domain.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_513_0.png)

**Figure 9-5 VLAN examples**

Switches are used to create VLANs, or separate broadcast domains. VLANs are not restricted to any physical boundary in the switched network, assuming that all the devices are interconnected via switches and that there are no intervening layer 3 devices. For example, a VLAN could be spread across multiple switches, or it could be contained in the same switch, as shown in Figure 9-6. This example shows three VLANs. Notice that VLANs are not tied to any physical location: PC-A, PC-B, PC-E, and PC-F are in the same VLAN but are connected to different ports of different switches. However, a VLAN could be contained to one switch, as PC-C and PC-D are connected to SwitchA.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_514_0.png)

**Figure 9-6 Physical switched topology using VLANs**

The switches in your network maintain the integrity of your VLANs. For example, if PC-A generates a broadcast, SwitchA and SwitchB will make sure that only other devices in that VLAN (PC-B, PC-E, and PC-F) will see the broadcast, and that other devices will not, and that holds true even across switches, as is the case in Figure 9-6.
