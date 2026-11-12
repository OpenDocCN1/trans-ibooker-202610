```
VAN-SW1(config-if)#exit
VAN-SW1(config)#vtp domain Lab101
Changing VTP domain name from NULL to Lab101
VAN-SW1(config)#vtp mode server
Device mode already VTP SERVER.
VAN-SW1(config)#vtp password vtppass
Setting device VLAN database password to vtppass
```

+   5. To verify the VTP settings, type the following commands:
+      VAN-SW1(config)#exit
+      VAN-SW1#show vtp status
+   6. Use the show vlan command to display the list of VLANs. Note that there are only default VLANs.
+   7. Use the following commands to create two VLANs:
+      VAN-SW1#config term
+      Enter configuration commands, one per line. End with CNTL/Z.
+      VAN-SW1(config)#vlan 2
+      VAN-SW1(config-vlan)#name Sales
+      VAN-SW1(config-vlan)#vlan 3
+      VAN-SW1(config-vlan)#name Mkt
+      VAN-SW1(config-vlan)#exit
+   8. Use the do show vlan command to display the list of VLANs. Note the two additional VLANs.
+   9. Console into VAN-SW2.
+   10. Use the show vlan command to display the VLANs. Note there are only default VLANs and there is no Sales or Mkt VLAN.
+   11. Use the show vtp status command to display VTP information. Note that the VTP domain is set, but the mode is server. You need to change this to client so that VLANs are not configured on this switch but can be received from the server (VAN-SW1).
+   12. Configure VAN-SW2 in VTP client mode so it can receive the list of VLANs from VAN-SW1 with the following commands:
+       VAN-SW2#config term
+       VAN-SW2(config)#vtp mode client
+       VAN-SW2(config)#vtp password vtppass
+   13. Use the show vtp status command to verify that the mode was changed to client.
+   14. Use the show vlan command to verify that the list of VLANs was received from the VAN-SW1 switch.
+   15. You can now place different interfaces into the Sales and Mkt VLANs on VAN-SW2.