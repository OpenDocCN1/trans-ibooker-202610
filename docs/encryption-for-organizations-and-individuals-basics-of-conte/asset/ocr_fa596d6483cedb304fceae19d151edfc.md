# Staying Safer in Linux

Now it’s time to take a deeper look into the Linux side of things. Although we took a glance at a few cryptographic solutions for this great operating system in Chapter 3, it’s time to review a couple more popular pieces of security software for this fine operating system.

# CryFS

Offering pretty much a complete cryptographic solution, *CryFS* is a wonderful piece of open source software for most distributions of Linux. Not only does it protect local data on your computer, but it can also be harnessed for cloud-based services, such as *DropBox*. CryFS makes sure only encrypted data leaves your system. This encompasses not only your files and directories, but metadata (including file size information) as well.

The encryption approach in CryFS actually consists of a double-layer process. First, it uses the most robust algorithm we currently have, the AES-256, for an external layer. After this the user can choose to implement a different encryption scheme for an inner layer. Both of these layers share the password given by the user. A truly unique solution, CryFS is highly recommended for more advanced cryptographers looking for the current state of the art in Linux security.

Let’s now go through the basic steps of getting CryFS running on your Linux distribution, shall we?

+   1. **Installation of CryFS for Ubuntu and Debian is simple.** Just enter the following into a terminal window: $ sudo apt install cryfs

+   2. **Next we create two new directories with CryFS.** Type in the following: $ cryfs realdir virtdir. You can naturally choose whatever names you want for both of these. The latter (i.e., *virtdir* in our example) is