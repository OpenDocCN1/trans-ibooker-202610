
Next, it’s time to choose a way for storing the recovery key. You’ll have the alternatives of using your Microsoft account, saving the recovery key to another USB device, saving it into a file, or printing it out on a piece of paper.

We’ll now get to choose how much of the drive will be encrypted. You can select to encrypt either only the files already in use or to cover the entire hard drive. In general, the latter is the slower, but safer option.

It’s at this point when installing BitLocker on Windows 10 you get the option of using the XTS-AES operating mode, referred on this screen as *New encryption mode*. If you’re encrypting a movable storage device, such as a USB-stick, it’s best to choose *Compatible mode* instead.

Finally, BitLocker will want to check your drive for errors and restart Windows to begin the encryption process, which will run smoothly in the background. If you need to get some work done or even shut down your computer, you can do it safely even at this stage of BitLocker’s operation.

The preceding example, while providing us with a robustly encrypted operating system, didn’t take into account the various hardware-based security measures previously mentioned in the chapter. All of that will be covered later in the book.

## Linux Unified Key Setup (LUKS)

Most current distributions of Linux offer a robust encryption standard, the *Linux Unified Key Setup*, created by one Clemens Fruhwirth and released in 2004. A very versatile system, LUKS offers three varieties of AES encryption in addition to many others. It allows for full-disk encryption, too. LUKS is used in conjunction with *cryptsetup*, a tool for creating and managing encrypted disks and partitions in Linux.

## DriveCrypt by SecurStar

Advertised as “military strength encryption,” DriveCrypt’s 1344-bit key strength sounds quite robust indeed. As for its algorithms, the software offers about a half a dozen including AES and, rather interestingly, the obsolete DES. How exactly the massive key strength is obtained is unclear, but on paper DriveCrypt seems like a solid Windows-only encryption solution. The software even features support for advanced steganography (as discussed earlier in the book) which is the technique of hiding sensitive data in plain daylight in the form of fake music files and the like.

As for backdoors, SecurStar remain adamant that these are not included in their products. Based in Germany, their claim seems to carry some heft as the country is quite comfortable with cryptographic freedoms. This may or may not change in the future, and if it does, SecurStar have vowed to “move the company accordingly.”

A free 30-day demo version of DriveCrypt is available as of this writing. Download DriveCrypt here: www.securstar.com/en/drivecrypt.html.

## eCryptfs

Not a software suite per se, eCryptfs is in fact a complete cryptographic file system for Linux. This solution is actually a so-called stacked file system, which refers to eCryptfs co-operating with a Linux file system. It has been a part of the Ubuntu distribution since version 9.04. It’s also included in Google’s Chrome Operating System, which is a Linux variant.

eCryptfs adds cryptographic metadata into files, so that any encrypted files can be easily transmitted between hosts. These files will be then decrypted automatically should the proper key/password be in the Linux keyring. eCryptfs utilizes the AES algorithm.

As with most things Linux, eCryptfs is open source and not likely to have backdoors. A tad complicated for beginners, this is a powerful cryptographic tool in the arsenal of any organization in need of additional reinforcement into their security regime.

Download eCryptfs here: https://ecryptfs.org.

## ProxyCrypt by v77

Described by its author as a tool for “paranoids and advanced users,” ProxyCrypt is a powerful open source encryption system. On offer are three algorithms, including the 256-bit variety of AES. Available for Windows only, ProxyCrypt is a somewhat obscure piece of software but is both speedy and secure. It’s a great choice for a robust full-disk encryption solution with features such as hardware acceleration and automatic key stretching (a technique discussed in the previous chapter).

Although ProxyCrypt was first released in 2013, it is a command-line tool, meaning it uses a sparse text-based approach. However, a free graphical user interface is available, which makes the software more appealing to beginners.

Download ProxyCrypt here: https://sourceforge.net/projects/proxycrypt.

## VeraCrypt by IDRIX

Based on the legacy of a once very popular cryptographic system, TrueCrypt (2004–2011), VeraCrypt is an open source, feature-rich encryption suite available for Windows, MacOS, and Linux. Extensively audited and deemed to be without backdoors, you can use this fine piece of software for either full-disk encryption or for container files.

VeraCrypt offers five individual encryption algorithms, including AES, as well as their ten different combinations. It uses the XTS mode of operation and has full support for hardware-based acceleration (i.e., AES-NI). Also, if you have old TrueCrypt file containers somewhere you still want to access, VeraCrypt allows for that, too. Do note that the developers of VeraCrypt are vehemently opposed to adding support for trusted platform modules, as they feel a potential physical visit from a malicious party would negate the benefits of any TPM-devices.

Download VeraCrypt here: [www.veracrypt.fr/en/Home.html](https://www.veracrypt.fr/en/Home.html).

## Tutorial Time!

Knowing how much theory goes into encryption, it might surprise you how comfortable much of current-generation cryptographic software is to use. We’ll now create a secure file container which may or may not be your very first one. For this task we’ll be using the outstanding VeraCrypt tool.

- 1. **Download VeraCrypt from the URL provided before.**
- 2. **Install and run the software.** You’ll arrive at the main VeraCrypt window (see Figure 4–1).

![](img/df9b6c40c665f58cab0c435444990c63_87_0.png)

## Figure 4-1. The main VeraCrypt-window

It is here we get to create and access our encrypted file containers. You’ll notice the software offers you a total of 64 slots. This is the maximum amount of file containers you can operate on at the same time. However, you might only need a slot or two, depending on the size of your operation/organization. Next, we’ll proceed to create a new container called topsecret (which is already visible in Figure 4-1).

- 3. Click Create Volume. You’ll be given two options. Choose Create an encrypted file container and click Next.
- 4. **On the next prompt, choose *Standard VeraCrypt volume* and click Next.**
- 5. **Click Select file and navigate to the folder you want our file container to reside in.**
- 6. **Review Encryption options.** The defaults are set to AES as the encryption algorithm and SHA-256 as the hashing method. These are both fine choices, so let’s just click *Next*.
- 7. **Decide on the volume size.** You can enter the file container size in kilobytes, megabytes, or gigabytes. For our purposes, let’s choose the nice round number of ten megabytes and click *Next*.
- 8. **Enter key/password.** VeraCrypt wisely informs us about the importance of good passwords. A password which isn’t directly taken from the dictionary and is at least 20 characters in length is highly recommended. A good password would be something like *iuyERwuy8321!53tNbv_*. A bad one would be *hello123*. A really bad one would be *hello*. Should you enter an atrocious password at this stage, the software will inform you of this and suggest you come up with a better one post haste. After you’ve decided on a password, click *Next*.

**Note** If you ever regret your choice of passwords in VeraCrypt, you can change them later from the main window by choosing a mounted file container and clicking Volume Tools ► Change Volume Password.
