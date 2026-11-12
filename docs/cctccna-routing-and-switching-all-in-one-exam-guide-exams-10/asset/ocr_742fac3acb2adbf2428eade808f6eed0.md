![](bbox=[0.12, 0.08, 0.9, 0.47])

+   1. Ensure that all physical components such as routers and switches are connected as per the network diagram, and then power on the devices.
+   2. Connect your workstation to the console port of the router that will become VAN-R1. Then launch PuTTY, choose Serial connection, set the correct COM port, and choose Open.
+   3. Press ENTER in the PuTTY window to open a prompt on your Cisco device.
+   4. Access VAN-R1’s Privileged EXEC mode and view the running configuration by using the following commands:
+   5. View the contents of NVRAM by using the **show startup-config** command.

```
VAN-R1>enable
VAN-R1#show running-config
```

Notice the hostname in running-config.

Notice that the hostname in startup-config is the same as the hostname setting in running-config.