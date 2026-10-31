We are presented with only a Command Prompt, and inside that prompt, it has auto-launched a configuration utility called **SConfig**. By using the keyboard here, I can do things such as set the hostname of this server, join it to a domain, and change networking settings. Once you have finished using this CLI interface to set the basic requirements on the server and get it communicating with the network, you really don't need to access the console of this Hyper-V Server again, unless you need to backtrack and revisit this configuration screen to change something. Instead, after configuring the Hyper-V Server, you simply utilize **Hyper-V Manager**, or PowerShell, on another server or desktop inside your network, to tap remotely into the management of VMs that are running on this Hyper-V Server.

In *Figure 14.40*, you can see that I have launched Hyper-V Manager. I am running this instance of Hyper-V Manager from my Windows 10 machine where I have the Hyper-V role installed. From here, I right-click on **Hyper-V Manager** and choose **Connect to Server....** I then input the name of my new Hyper-V Server, and the console creates a remote connection. From this remote connection, I can now utilize all functionality inside my Window 10 Hyper-V Manager as if I were logged straight into the new Hyper-V Server:

![](bbox=[0.13, 0.42, 0.87, 0.67])

Figure 14.40: Remotely connecting to a Hyper-V Server

Similar to the way that most tasks performed on Server Core or Nano Server are handled remotely – through the use of remote consoles or PowerShell – we make all ongoing maintenance and administration of this Hyper-V Server happen from a remote Hyper-V Manager console.

Hyper-V Server gives you the security benefits of a GUI-less interface, combined with the flexibility benefits of hosting an unlimited number of virtual machines, at a price point that nobody can argue with!