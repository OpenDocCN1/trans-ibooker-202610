
**Create a nonadministrator account for trivial use.** Administrators are great and necessary people, but an administrator’s account falling into the wrong hands can be a disaster. Therefore, it makes sense to have a secondary account without administrator privileges. For this, you should navigate to *Users & Groups* in your Mac’s *System Preferences*. Also, make sure to disable automatic login from *Login Options* (see Figure 6-6).

![](img/df9b6c40c665f58cab0c435444990c63_148_0.png)

*Figure 6-6. The MacOS user settings screen. Note Login Options in the bottom left*

## T2, Judgment Chip: TPM, Apple Style

With the release of the mighty 2017 *iMac Pro*, Apple has been including a hybrid TPM/accelerator chip in many of their computers. The T2 chip is Apple’s second-generation custom microchip used for on-the-fly encryption and several other purposes. Its functions include dedicated audio management, webcam-related image processing, and cryptographic functions for one’s solid-state drive(s). The T2 runs on a discrete operating system known as *BridgeOS* with which the average user rarely needs to tamper with. Having a T2 in your Mac not only makes it more secure, but also slightly faster as quite a number of tasks are offloaded from the CPU onto this chip. The only downside is it’s pretty much impossible to recover data from a Mac with a failed T2 chip as the system is quite impenetrable.

All T2-equipped Macs are issued a *Mac Unique ID (UID)* and a *Device Group ID (GID)* during manufacturing. Both the UID and GID are tied to a particular device only. These strings are inaccessible by any software or hardware outside of their environment, which Apple calls the *Secure Enclave*. The *UID* and *GID* are used by the cryptographic engine in a T2 device. To the outside world, this engine shows only the results of its encryption or decryption operations. Not even Apple or their suppliers can later access the UID contents directly. This is the first layer of security in a T2 chip.

Now, when FileVault is enabled in MacOS, your SSD-drives are quite safe from prying eyes combined with Apple’s custom chip. The T2 chip’s cryptographic functions operate under FileVault using the rock-solid AES-256 algorithm using the XTS operating mode as discussed in Chapter 4. As the encryption/decryption process is also completely offloaded onto Apple’s custom chip, there is no longer any additional speed penalty to any of your solid-state disks. Even when FileVault is disabled on a T2-equipped Mac, the SSDs in the system remain encrypted. However, they are then only protected by the hardware UID. As all the drives are subjected to encryption in every case, switching FileVault on later isn’t a time-consuming process and is highly recommended for the extra layer of security it provides.

As of Q2 2020, the following Apple computers come with the T2 chip onboard:

- Desktops: iMac Pro (2017), Mac Mini (2018), Mac Pro (2019), and presumably later models
- Laptops: MacBook Air (2018), MacBook Pro (2018), and later models

Not sure if a particular Mac has the T2 chip or just feel like perusing some information concerning one? Open *About this Mac* and click *System Report*. Select the *Hardware* tab. If it has a subtab called *Controller* also mentioning the chip, you’re in luck.

### T2 Maintenance 101

As with traditional TPM modules, issues may turn up which might require you to refresh your T2 chip settings. Apple has integrated its *System Management Controller (SMC)* within the T2; therefore, if your computer has sudden issues with its cooling fans, external ports, or power management, a reset of the SMC might be in order. We’ll now review the procedure for newer T2-equipped Macs.

For your Apple desktop, first shut your computer down. Disconnect its power lead for 15 seconds. After this reconnect the power lead and wait for five seconds. Finally, press the power button like you normally would to start up your Mac.

As for T2-equipped Apple laptops, the process to reset the chip is a tad more complex. First, shut down your MacBook. Then press and hold the *right Shift*, *left Option*, and *left Control* keys for at least seven seconds. Continue by holding those keys while you also press and hold the power button for yet another seven seconds. Now, release the keys and the power button, wait for a couple of seconds, and finally just power up the laptop.

## Security Software for Mac

We’ll now review some more security solutions for your MacOS-based environments.

- **AppCrypt by Cisdem** (30/45 USD for a lifetime home/business license)

  A tool catered perfectly not just for worried parents but also for the organizational clientele, *AppCrypt* works to block specific websites. Bid farewell to your employees wasting time on *Facebook* and *Twitter* with this Mac-specific solution. In addition to websites, you also get to prohibit the use of specific applications you’re not particularly fond of, such as games and other time-consumers, with the use of a master password only you should be aware of. AppCrypt makes it somewhat hard to circumvent its blocking prowess by forcing said master password on MacOS *System Preferences*, *Terminal*, *Activity Monitor*, and *Console*.

  AppCrypt has scheduling functionality, so you can set the exact date and time to lift your app usage bans. This feature won’t work for website blocking, however. AppCrypt is available for MacOS versions 10.10 (Yosemite) or newer.

- Discover AppCrypt here and download a demo version: www.cisdem.com/appcrypt-mac.html.

- **Encrypto by MacPaw**

  Sometimes just a few of your files need that extra layer of security. *Encrypto* is a nifty and free solution for securing your individual files and folders on the MacOS. Relying on the 256-bit variety of AES, it offers without a doubt robust cryptographic capabilities. Not only that, you can share your encrypted files with Windows users, as Encrypto is also available for Microsoft’s range of operating systems starting from *Windows XP*. On a Mac the software also allows for easy integration into your OS with an additional Share button that’s there for sending your files over to *iMessage* or *AirDrop*. Support for decrypting .crypt-files in iOS devices is lacking as of early 2020.

  The program features a clean, user-friendly graphical user interface so no typing into terminal windows is needed to create a very secure file container with a .crypto extension. By simply dragging and dropping a file into the program window and entering a password is all it takes.

  With Encrypto you can also add handy password hints into each file should you choose to; these may be especially handy when sharing data. There is the minor annoyance of a window requesting you share “anonymous user data,” whatever that means, every time you shut *Encrypto* down. However, this shouldn’t dissuade you from trying out this fine piece of cryptographic software.

- Download Encrypto here: https://macpaw.com/encrypto.

## Staying Safer in Linux

Now it’s time to take a deeper look into the Linux side of things. Although we took a glance at a few cryptographic solutions for this great operating system in Chapter 3, it’s time to review a couple more popular pieces of security software for this fine operating system.

### CryFS

Offering pretty much a complete cryptographic solution, *CryFS* is a wonderful piece of open source software for most distributions of Linux. Not only does it protect local data on your computer, but it can also be harnessed for cloud-based services, such as *DropBox*. CryFS makes sure only encrypted data leaves your system. This encompasses not only your files and directories, but metadata (including file size information) as well.

The encryption approach in CryFS actually consists of a double-layer process. First, it uses the most robust algorithm we currently have, the AES-256, for an external layer. After this the user can choose to implement a different encryption scheme for an inner layer. Both of these layers share the password given by the user. A truly unique solution, CryFS is highly recommended for more advanced cryptographers looking for the current state of the art in Linux security.

Let’s now go through the basic steps of getting CryFS running on your Linux distribution, shall we?

1. **Installation of CryFS for Ubuntu and Debian is simple.** Just enter the following into a terminal window: `$ sudo apt install cryfs`
2. **Next we create two new directories with CryFS.** Type in the following: `$ cryfs realdir virtdir`. You can naturally choose whatever names you want for both of these. The latter (i.e., *virtdir* in our example) is
