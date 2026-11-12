+   2. Use the **erase startup-config** command to delete your configuration file in NVRAM.
+   3. **B and F.** The **show startup-config** command displays the saved configuration found in NVRAM. If no configuration is stored in this memory location, you see the “%%Non-volatile configuration memory is not present” message.
+   4. **A, C, and D.** When you use the **copy flash tftp** command, you are prompted for the TFTP server’s IP address, the source filename of IOS in flash, and the name you want to call the IOS image on the TFTP server.
+   5. **C.** The **show version** command will display the current software version your IOS device is running.
+   6. ```
+   line vty 0 4
+     transport input ssh
+     login local
+   ```
+   7. **crypto key generate rsa**
+   8. **D.** CDP can be used to validate that you have layer 2 connectivity with a connected device.
+   9. **D.** The **traceroute** command tests layer 3.
+   10. **B.** Use CTRL-SHIFT-6, x to suspend a telnet session.
+   11. **D.** Use the **show version** or **show licenses** command to display the licenses activated on an IOS device.
+   12. **C.** The **show cdp neighbor** command is used to display a list of neighboring devices detected with CDP and characteristics such as the ports used to connect to that device and the capabilities of the device.
+   13. **A.** The **show cdp neighbor detail** command is used to list out the characteristics of neighboring devices including the IP address of the device, the platform, capabilities, and the IOS version.
+   14. **B.** LLDP is an industry standard discovery protocol that is supported by many vendors of network devices (CDP is only for Cisco devices). To use LLDP on your Cisco device to discover other devices, you