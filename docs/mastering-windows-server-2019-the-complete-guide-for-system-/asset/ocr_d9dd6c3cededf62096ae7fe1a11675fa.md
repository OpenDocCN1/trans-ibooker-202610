# Hyper-V console, Remote Desktop Protocol (RDP), or PowerShell

While hardware adjustments to VMs need to be made through Hyper-V Manager, your daily interaction with these VMs running as servers in your environment does not necessarily mean you have to log in to your Hyper-V Server. If you happen to be inside Hyper-V Manager anyway, you can quickly and easily use that Connect function to interact with the console of your servers, through the use of the Hyper-V console tool. Accessing your servers this way is beneficial if you need to see something in the BIOS, or otherwise outside of the Windows operating system that is running on that VM, but it's not often that you require this level of console access.

When you have Windows Servers running as VMs, it is much more common to interact with these servers in the same way that you would interact with physical servers on your network. While I have been accessing my WEB3 server through the Hyper-V console in this chapter, now that I have Windows Server 2019 installed on WEB3 and I have enabled the RDP capabilities on it, there is no reason why I couldn't just pop open MSTSC and log into WEB3 that way, straight from my desktop:

![](bbox=[0.13, 0.4, 0.87, 0.62])

Figure 14.34: Using normal support tools to connect to VMs

The same is true for PowerShell or any other traditional way of remotely accessing services on any other server. Since this VM is fully online and has the server operating system installed, I can use PowerShell remoting to manipulate my WEB3 server as well, from another server or from my desktop computer. Once you are finished building out the hardware and installing the operating system on a VM, it's rare that you actually need to use the Hyper-V console to interact with that server. The primary reasons for opening up Hyper-V Manager to reach a VM are to make hardware-level changes on that server, such as adding a hard drive, adjusting RAM, or moving a network connection from one switch to another.