Once you have your .ISO file for the Windows Server 2019 installation downloaded onto your computer, insert a fresh DVD into your disc drive and browse to the new file. Simply right-click on the .ISO file, and then choose your menu option for Burn disc image. This launches a simple wizard that will extract and burn your new .ISO file the correct way onto the DVD, making it a bootable installation media for your new server, as shown in Figure 2.1:

![](bbox=[0.13, 0.2, 0.87, 0.37])

Figure 2.1: Burning your .ISO file onto DVD

It is probable when you attempt to download Windows Server 2019 and use this Windows Disc Image Burner utility with a DVD that you grabbed off your stack of standard blank DVDs, that you will receive the following error message: The disc image file is too large and will not fit on the recordable disc.

This should come as no surprise because our operating system installer files have been getting larger and larger over the years. We have now reached the critical tipping point where the standard Server 2019 ISO installer is larger than a standard 4.7 GB DVD disc. To burn this ISO onto a DVD, you will need to hit the store and find some dual-layer discs that can handle more data.

# Creating a bootable USB stick

DVDs can be cumbersome and annoying, and now they are also too small for our purposes. Therefore, when installing the newer, larger operating systems it is becoming commonplace to prep a USB stick to use for the installation of the operating system, rather than relying on a DVD.

To do this, all you need is a Windows computer, a USB stick that is at least 8 GB, and access to the internet. You will need to download the same ISO that we discussed earlier, as that contains all of the installation files for Server 2019. Then you will also need to download and install some kind of bootable USB creation tool. There are various free ones available (Rufus is pretty popular), but the one straight from Microsoft is called the Windows 7 USB/DVD Download Tool. Why does it have this crazy name that includes the words Windows 7 right in it? Don't ask me.