VIDEO 7.03. The digital resources that accompany this book contain a multimedia demonstration of configuring a hostname on your Cisco device.

# Subconfiguration Modes

Certain configuration commands on IOS devices take you into a specific subconfiguration mode, commonly referred to as a *subcommand mode*. Table 7-2 shows a few subconfiguration modes that you may see on IOS devices.

| IOS Prompt | Subconfiguration Mode |
|---|---|
| (config-if)# | Interface subconfiguration mode |
| (config-subif)# | Subinterface subconfiguration mode |
| (config-line)# | Line subconfiguration mode |
| (config-controller)# | Controller subconfiguration mode |
| (config-router)# | IP routing protocol subconfiguration mode |

Table 7-2 Subconfiguration Modes on IOS Devices

![](bbox=[0.14, 0.59, 0.26, 0.67])

**NOTE** Not all subconfiguration modes are supported on all IOS devices. When you are working in a subconfiguration mode, the commands you enter affect only a specific component of the router or switch.

To leave a subconfiguration mode and return to Global Configuration mode, use the **exit** command. Using the **end** command or pressing CTRL-Z will always take you back to privileged EXEC mode no matter what