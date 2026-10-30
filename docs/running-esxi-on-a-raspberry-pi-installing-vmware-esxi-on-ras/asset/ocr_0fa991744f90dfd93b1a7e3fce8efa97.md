![](bbox=[0.09, 0.12, 0.87, 0.38])

# Figure 6-2. Session Timeout

There are many other values under the Advanced settings tab, but it is best not to change these unless you are implicitly instructed to do so as serious and unexpected results may occur.

# Starting VMs Automatically

One of the settings that I have found useful is to have VMs start up automatically when the ESXi server starts. This comes in handy when the ESXi goes down due to a power outage or other unexpected events, especially for VMs that host your critical applications like your active directory (AD) services.

To have a VM automatically start, select the System tab, and click Autostart. Figure 6-3 will look different than what you will see on your system at this point as you haven’t created any VMs yet, but this screenshot shows how you can enable autostart for a VM and then specify a delay in the startup and shutdown times for it. These times are useful when you want one VM that is dependent on another to start before or after the other. A prime example of this is having a VM with a database start before a VM that hosts an application that depends on the database.