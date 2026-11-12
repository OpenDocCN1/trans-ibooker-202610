![](bbox=[0.14, 0.11, 0.24, 0.17])

**TIP** If you think your **debug** commands are causing performance problems, use the **show processes cpu** command to check your CPU utilization for the device’s various processes, including **debug.**

# Exercise 8-6: Using the Router’s Troubleshooting Tools

In this exercise you will practice using some of the troubleshooting tools available in the Cisco IOS. Note that this exercise is using the same network topology used in the last exercise.

+   1. Console into router VAN-R1 and use the **show cdp neighbors** command to see what neighboring devices are connected to VAN-R1. Record them here:
+   
+   Device ID: ________________
+   Local Interface: ________________
+   Platform: ________________
+   Capability: ________________
+   Remote Port: ________________
+   
+   Note that you cannot see the IP address of the neighboring device.
+   
+   2. To see the IP address of the neighboring device with CDP, use the **show cdp neighbors detail** command. Record the information here:
+   
+   Device ID: ________________
+   IP Address: ________________
+   IOS Version: ________________
+   
+   3. Now that you know the IP address of the neighboring device, you could telnet into it and then discover other devices from that