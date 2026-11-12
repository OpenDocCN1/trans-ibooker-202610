
If you're not sure whether your system is using UEFI, you can easily check. After the Linux system boots, issue the command `ls /sys/firmware/efi`. If you receive a no such file or directory message, then you're employing BIOS. On the other hand, if you see files, then your system booted using UEFI.

## Looking at Boot Loaders

The boot loader program helps bridge the gap between the system firmware and the full Linux operating system kernel. In Linux, there are several choices of boot loaders to use, which are covered in this section.

### Boot Loader Principles

The first version of the GRand Unified Bootloader (GRUB) boot loader (now called _GRUB Legacy_) was created in 1999 to provide a robust and configurable boot loader. GRUB quickly became the default boot loader for all Linux distributions, whether they were run on BIOS or UEFI systems.

_GRUB2_ was created in 2005 as a total rewrite of the GRUB Legacy system. It supports advanced features, such as the ability to load hardware driver modules and using logic statements to alter the boot menu options dynamically, depending on conditions detected on the system (such as if an external hard drive is connected).

### Using GRUB Legacy as the Boot Loader

The GRUB Legacy boot loader was designed to simplify the process of creating boot menus and passing options to kernels. GRUB Legacy allows you to select multiple kernels and/or operating systems using both a menu interface as well as an interactive shell. With the menu interface, you configure options for each kernel or operating system you wish to boot up. With the interactive shell, you can customize boot commands on the fly.

Both the menu and the interactive shell utilize a set of commands that control features of the boot loader. This section walks you through how to configure the GRUB Legacy boot loader, how to install it, and how to interact with it at boot time.

### Configuring GRUB Legacy

When you use the GRUB Legacy interactive menu, you need to tell it what options to show. You do that by using special GRUB _menu commands_.

The GRUB Legacy system stores the menu commands in a standard text configuration file called _menu.lst_; it is stored in the `/boot/grub/` directory. (Though not a requirement, some Linux distributions create a separate `/boot` partition on the hard drive.) Red Hat–derived Linux distributions (such as CentOS and Fedora) use _grub.conf_ instead of _menu.lst_ for the configuration file. Also, you may find that the menu.lst file is symbolically linked to the grub.conf file.

The GRUB Legacy configuration file consists of two sections:

- Global definitions
- Operating system boot definitions

The global definitions section defines commands that control the overall operation of the GRUB Legacy boot menu. The global definitions must appear first in the configuration file. There are only a handful of global settings that you can make. Table 5.1 shows these settings.

### TABLE 5.1 GRUB Legacy global commands

| Setting | Description |
| :--- | :--- |
| color | Specifies the foreground and background colors to use in the boot menu |
| default | Defines the default menu option to select |
| fallback | A secondary menu selection to use if the default menu option fails |
| hiddenmenu | Don't display the menu selection options |
| splashimage | Points to an image file to use as the background for the boot menu |
| timeout | Specifies the amount of time to wait for a menu selection before using the default |

For GRUB Legacy, to define a value for a command, you just list the value as a command-line parameter:

```
default 0
timeout 10
color white/blue yellow/blue
```

The color command defines the color scheme for the menu. The first pair of colors defines the foreground/background for normal menu entries, and the second pair defines the foreground/background for the selected menu entry.

After the global definitions, you place definitions for the individual operating systems that are installed on the system. Each operating system should have its own definition section. There are a lot of boot definition settings that you can use to customize how the boot loader finds the operating system kernel file. Fortunately, only a few commands are required to define the operating system. The ones to remember are as follows:

- Title: The first line for each boot definition section, this is what appears in the boot menu.
- Root: Defines the disk and partition where the GRUB /boot folder partition is located on the system.
- Kernel: Defines the kernel image file stored in the /boot folder to load.
- Initrd: Defines the initial RAM disk file or filesystem, which contains drivers necessary for the kernel to interact with the system hardware.
- Rootnoverify: Defines non-Linux boot partitions, such as Windows.

The root command defines the hard drive and partition that contains the /boot folder for GRUB Legacy. Unfortunately, GRUB Legacy uses a somewhat odd way of referencing those values:

```
(hddrive, partition)
```

Also unfortunately, GRUB Legacy doesn't refer to hard drives the way Linux does; it uses a numbering system to reference both disks and partitions, starting at 0 instead of at 1. For example, to reference the first partition on the first hard drive of the system, you'd use (hd0,0). To reference the second partition on the first hard drive, you'd use (hd0,1).

The initrd command is another important feature in GRUB Legacy. It helps to solve a problem that arises when using specialized hardware or filesystems as the root drive. The initrd command defines a file that's mounted by the kernel at boot time as a RAM disk or filesystem. The kernel can then load modules from the RAM disk or filesystem, which then allows it to access hardware or filesystems not compiled into the kernel itself.

> Before Linux kernel v2.6, the initial RAM disk was used to hold kernel modules needed at boot time. Since that time, it has been replaced with the initial RAM filesystem (initramfs). It can be a little confusing, because the initrd command within the GRUB configuration file denotes either an initial RAM disk or an initial RAM filesystem. Which one used depends on your distribution and its age.

### Listing 5.3: Sample GRUB Legacy configuration file

```
default 0
timeout 10
color white/blue yellow/blue

title CentOS Linux
root (hd1,0)
kernel (hd1,0)/boot/vmlinuz
initrd /boot/initrd

title Windows
rootnoverify (hd0,0)
```

This example shows two boot options: one for a CentOS Linux system and one for a Windows system. The CentOS system is installed on the first partition of the second hard drive, and the Windows system is installed on the first partition of the first hard drive. The Linux boot selection specifies both the kernel file to load as well as the initrd image file to load into memory.

## Installing GRUB Legacy

After you build the GRUB Legacy configuration file, you must install the GRUB Legacy program in the MBR. The command to do this is `grub-install`.

The `grub-install` command uses a single parameter that indicates the partition on which to install GRUB. You can specify the partition using either the Linux or the GRUB Legacy format. For example, to use the Linux format, you'd type

```
# grub-install /dev/sda
```

To install GRUB on the MBR of the first hard drive and use the GRUB Legacy format, you must enclose the hard drive format in quotes:

```
# grub-install '(hd0)'
```

If you're using the chainloading method and prefer to install a copy of GRUB Legacy on the boot sector of a partition instead of to the MBR of a hard drive, you must specify the partition, again using either the Linux or the GRUB format:

```
# grub-install /dev/sda1
# grub-install '(hd0,0)'
```

After making changes to the GRUB Legacy configuration file, you don't need to reinstall GRUB Legacy in the MBR. GRUB Legacy reads the configuration file each time it runs.

## Interacting with GRUB Legacy

When you boot a system that uses the GRUB Legacy boot loader, you'll see a menu that shows the boot options that you defined in the configuration file. If you wait for the time-out to expire, the default boot option will process. Alternatively, you can use the arrow keys to select one of the boot options and then press the Enter key to select it.

You can also edit boot options on the fly from the GRUB menu:

- 1. Use the arrow key to move to the boot option you want to modify, and then press the E key.
- 2. Use the arrow key to move the cursor to the line that you need to modify, and then press the E key to edit it.
- 3. Press the B key to boot the system using the new values.

You can also press the C key at any time to enter an interactive shell mode (also called GRUB command line), allowing you to submit GRUB commands on the fly. You can leave the GRUB command line, and return to the menu, by pressing the ESC key.
