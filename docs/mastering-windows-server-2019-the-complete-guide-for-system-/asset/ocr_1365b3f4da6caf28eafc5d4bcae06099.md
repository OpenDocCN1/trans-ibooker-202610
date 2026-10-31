Now that our new VM has been started, *what can we expect to see inside the console window?* A boot failure error, of course:

![](bbox=[0.13, 0.13, 0.87, 0.27])

Figure 14.18: The initial start of this VM was not so successful

# Installing the operating system

We get a boot failure message because we didn't specify any operating system media during our wizard, and so Hyper-V has created our VM and our new hard disk, but just like when you build a new server out of fresh hardware, you need software to be installed on that hard disk in order for it to do something. Luckily, installing an operating system on a VM is even easier than installing it on a physical server. Heading back into the **Hyper-V Manager** console, right-click on the name of your new VM and go to **Settings....**

Inside **Settings**, you will see that this VM has a **DVD Drive** automatically listed in **IDE Controller 1**. If you click on **DVD Drive**, you can easily tell it to mount any ISO to that drive. Copy the ISO file of the operating system installer you wish to run onto the hard drive of your Hyper-V Server.

I typically place all of my ISOs inside a dedicated folder called ISOs, right alongside my VMs folder, and then **Browse...** to it from this screen. Connecting an ISO to your VM is the same as if you were plugging a physical installation DVD into a physical server: