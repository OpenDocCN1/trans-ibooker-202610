As you can see in Figure 14.5, the default locations for storing these items are something you would expect to see in a client application you were installing on a laptop, but you wouldn't expect something as heavy as Hyper-V to be storing its core files in a shared-user Documents folder. I suppose since Microsoft doesn't know the configuration of your server, it can't make any real guesses as to where you want to really store that data, and so it sets the default to be something that would work technically but should probably be changed as a matter of best practice. Many Hyper-V Servers will have dedicated storage, even if only a separate hard disk, on which these files are planned to be stored. Make sure you take a minute on this screen and change the default storage locations of your VM files:

![](bbox=[0.13, 0.28, 0.87, 0.54])

Figure 14.5: Defining default storage locations for VM files

> Remember that the version of Windows Server 2019 you are running determines how many VMs you will be able to run on top of this host. Server 2019 Standard limits you to running two VMs, while the Datacenter edition gives you access to launch as many as you can fit on the hardware.

# Using virtual switches

Upon completion of the Hyper-V role installation, your first inclination may be to jump right in and start creating VMs, but you should really take a minute to make sure that the networking capabilities of your Hyper-V Server are adequate to meet your needs. During the role-installation process, we selected the physical NICs that are to be passed through into Hyper-V, and that screen told us it was going to establish a virtual switch for each of these NICs. But what does that look like inside the console? And what options do we have for establishing networking between our virtual machines?