# Looking at Firmware

All IBM-compatible workstations and servers utilize some type of built-in firmware to control just how the installed operating system starts. On older workstations and servers, this firmware was called the Basic Input/Output System (BIOS). On newer workstations and servers, a new method called the Unified Extensible Firmware Interface (UEFI) maintains the system hardware status and launches an installed operating system.

# The BIOS Startup

The BIOS firmware found in older workstations and servers was somewhat limited in what it could do. The BIOS firmware had a simple menu interface that allowed you to change some settings to control how the system found hardware and define what device the BIOS should use to start the operating system.

One limitation of the original BIOS firmware was that it could read only one sector's worth of data from a hard drive into memory in order to run. As you can probably guess, that's not enough space to load an entire operating system. To get around that limitation, most operating systems (including Linux and Microsoft Windows) split the boot process into two parts.

First, the BIOS runs a boot loader (sometimes written as bootloader) program. The boot loader is a small program that initializes the necessary hardware to find and run the full operating system, usually found at another location on the same hard drive but sometimes situated on a separate internal or external storage device.

The boot loader program usually has a configuration file, so you can tell it where to look to find the actual operating system file to run or even to produce a small menu allowing the user to choose between multiple operating systems.

To get things started, the BIOS must know where to find the boot loader program on an installed storage device. Most BIOS setups allow you to load the boot loader program from several locations:

+   - An internal hard drive
+   - An external hard drive
+   - A CD/DVD drive
+   - A USB flash drive
+   - A network server

When booting from a hard drive, you must designate which hard drive, and from which partition on the hard drive the BIOS should load the boot loader program. This is done by defining a master boot record (MBR).

The MBR is the first sector on the first hard drive partition on the system. There is only one MBR for the computer system. The BIOS looks for the MBR and reads the program stored there into memory. Since the boot loader program must fit in one sector, it must be very small, so it can't do too much. The boot loader program mainly points to the location