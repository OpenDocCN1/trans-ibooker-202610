These days just about all IBM-compatible desktop and server systems utilize the UEFI firmware standard.

Instead of relying on a single boot sector on a hard drive to hold the boot loader program, UEFI specifies a special disk partition, called the *EFI System Partition* (ESP), to store boot loader programs. This allows for any size of boot loader program, plus the ability to store multiple boot loader programs for multiple operating systems.

The ESP setup utilizes the old Microsoft File Allocation Table (FAT) filesystem to store the boot loader programs. On Linux systems, the ESP is typically mounted in the /boot/efi directory, and the boot loader files are typically stored using the .efi filename extension, such as linux.efi.

The UEFI firmware utilizes a built-in mini boot loader (sometimes referred to as a boot manager) that allows you to configure which boot loader program file to launch.

Not all Linux distributions support the UEFI firmware. If you're using a UEFI system, ensure that the Linux distribution you select supports it.

With UEFI you need to register each individual boot loader file you want to appear at boot time in the boot manager interface menu. You can then select the boot loader to run each time you boot the system.

After the firmware finds and runs the boot loader, its job is done. The boot loader step in the boot process can be somewhat complicated; the next section dives into covering that.

# # Device Interfaces

Each device you connect to your Linux system uses some type of standard protocol to communicate with the system hardware. The Linux kernel software must know how to send data to and receive data from the hardware device using those protocols. There are currently three popular standards used to connect devices.

# ## PCI Boards

The Peripheral Component Interconnect (*PCI*) standard was developed in 1993 as a method for connecting hardware boards to PC motherboards. The standard has been updated a few times to accommodate faster interface speeds, as well as increasing data bus sizes on motherboards. The *PCI Express* (*PCIe*) standard is currently used on most server and desktop workstations to provide a common interface for external hardware devices.

Lots of different client devices use PCI boards to connect to a server or desktop workstation:

+   - **Internal hard drives:** Hard drives using the *Serial Advanced Technology Attachment* (*SATA*) and the *Small Computer System Interface* (*SCSI*) interface often use PCI boards to connect with workstations or servers. The Linux kernel automatically recognizes both SATA and SCSI hard drives connected to PCI boards.