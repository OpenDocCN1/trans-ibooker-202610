| Parameter | Description |
|---|---|
| rootflags= | Set root filesystem’s mount options |
| rw | Mount root filesystem as read-write |
| selinux | Disable SELinux at boot time |
| single, Single, 1, or S | Boot a SysVinit system to single-user mode |
| systemd.unit= | Boot a systemd system to specified target |

If you’d like to look at a complete list of kernel parameters, one is available at www.kernel.org/doc/Documentation/admin-guide/kernel-parameters.txt.

# Using Alternative Boot Loaders

Although GRUB Legacy and GRUB2 are the most popular Linux boot loader programs, you may run into a few others, depending on which Linux distributions you are using.

A legacy boot loader that predates GRUB Legacy is LILO. It’s inability to handle UEFI systems as well as other difficulties brought its development to an end in December 2015.

The systemd-boot loader program is starting to gain popularity in Linux distributions that use the systemd initialization method (covered later in this chapter). This boot loader generates a menu of boot image options and can load any UEFI boot image.

The U-Boot boot loader (also called Das U-Boot) can boot from any type of disk. It can load any type of boot image.

The SYSLINUX project includes five separate boot loader programs that have special uses in Linux:

+   - SYSLINUX: A boot loader for systems that use the Microsoft FAT filesystem (popular for booting from USB flash drives)
+   - EXTLINUX: A mini-boot loader for booting from an ext2, ext3, ext4, or btrfs filesystem
+   - ISOLINUX: A boot loader for booting from a LiveCD or LiveDVD
+   - PXELINUX: A boot loader for booting from a network server
+   - MEMDISK: A utility for booting older DOS operating systems from the other SYSLINUX project boot loaders