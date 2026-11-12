

| Parameter | Description |
|---|---|
| rootflags= | Set root filesystem’s mount options |
| rw | Mount root filesystem as read-write |
| selinux | Disable SELinux at boot time |
| single, Single, 1, or S | Boot a SysVinit system to single-user mode |
| systemd.unit= | Boot a systemd system to specified target |

If you’d like to look at a complete list of kernel parameters, one is available at www.kernel.org/doc/Documentation/admin-guide/kernel-parameters.txt.

## Using Alternative Boot Loaders

Although GRUB Legacy and GRUB2 are the most popular Linux boot loader programs, you may run into a few others, depending on which Linux distributions you are using.

A legacy boot loader that predates GRUB Legacy is LILO. Its inability to handle UEFI systems as well as other difficulties brought its development to an end in December 2015.

The systemd-boot loader program is starting to gain popularity in Linux distributions that use the systemd initialization method (covered later in this chapter). This boot loader generates a menu of boot image options and can load any UEFI boot image.

The U-Boot boot loader (also called Das U-Boot) can boot from any type of disk. It can load any type of boot image.

The SYSLINUX project includes five separate boot loader programs that have special uses in Linux:

- SYSLINUX: A boot loader for systems that use the Microsoft FAT filesystem (popular for booting from USB flash drives)
- EXTLINUX: A mini-boot loader for booting from an ext2, ext3, ext4, or btrfs filesystem
- ISOLINUX: A boot loader for booting from a LiveCD or LiveDVD
- PXELINUX: A boot loader for booting from a network server
- MEMDISK: A utility for booting older DOS operating systems from the other SYSLINUX project boot loaders

Since UEFI can load any size of boot loader program, it’s now possible to load a Linux operating system kernel directly without a special boot loader program. This feature was incorporated in the Linux kernel starting with version 3.3.0. However, this method isn’t common, as boot loader programs can provide more versatility in booting, especially when working with multiple operating systems.

## The Initialization Process

After your Linux system has traversed the boot process, it enters final system initialization, where it needs to start various services. A service, or daemon, is a program that performs a particular duty.

The initialization daemon (init) determines which services are started and in what order. This daemon also allows you to stop and manage the various system services. There are two initialization daemons with which you should be familiar:

**SysVinit** The SysVinit (SysV) was based on the Unix System V initialization daemon. Though it is not used by many major Linux distributions anymore, you still may find it lurking around that older Linux server at your company.

**systemd** The systemd initialization method is the new kid on the block. Started around 2010, it is now the most popular system service initialization and management mechanism. This daemon reduces initialization time by starting services in a parallel manner.

Before we start examining these system initialization daemons, it’s a good idea to take a look at the init program itself. Classically, service startups are handled by the init program. This program can be located in the /etc/, the /bin/, or the /sbin/ directory. Also, it typically has a process ID (PID) of 1.

This information will assist you in determining which system initialization method your current Linux distribution is using—systemd or SysVinit. First find the init program’s location using the which command. An example is shown in Listing 5.6.

### Listing 5.6: Finding the init program file location

```
# which init
/sbin/init
#
```

Now that you know the init program’s location, using super user privileges you can utilize the readlink -f command to see if the program is linked to another program, as shown in Listing 5.7.

### Listing 5.7: Checking the init program for links

```
# readlink -f /sbin/init
/usr/lib/systemd/systemd
#
```

You can see in Listing 5.7 that this system is actually using systemd. You can verify this by taking a look at PID 1, as shown in Listing 5.8.

### Listing 5.8: Checking PID 1

```
# ps -p 1
PID TTY          TIME CMD
1 ?        00:00:06 systemd
#
```

In Listing 5.8, the ps utility is used. This utility allows you to view processes. A process is a running program. The ps command shows you what program is running for a particular process in the CMD column. In this case, the systemd program is running. Thus, this Linux system is using systemd.

The init program or systemd is the parent process for every service on a Linux system. If your system has the pstree program installed, you can see a diagram depicting this relationship by typing pstree -p 1 at the command line.

Keep in mind that these discovery methods are not foolproof. Also, there are other system initialization methods, such as the now-defunct Upstart that used the initctl utility as its daemon interface. The following brief list shows a few Linux distribution versions that used Upstart:

- Fedora v9–v14
- openSUSE v11.3–v12.2
- RHEL v6
- Ubuntu v6.10–v15.04

If you are using the distribution versions recommended in Chapter 1, know that those distributions are all systemd systems.

## Using the systemd Initialization Process

The systemd approach introduced a major paradigm shift in how Linux systems manage services. Services can now be started when the system boots, when a particular hardware component is attached to the system, when certain other services are started, and so on. Some services can be started based upon a timer.

### Exploring Unit Files

The easiest way to start exploring systemd is through the _systemd units_. A unit defines a service, a group of services, or an action. Each unit consists of a name, a type, and a configuration file. There are currently 12 different systemd unit types:

- automount
- device
- mount
- path
- scope
- service
- slice
- snapshot
- socket
- swap
- target
- timer

The _systemctl_ utility is the main gateway to managing systemd and system services. Its basic syntax is as follows:

```
systemctl [OPTIONS...] COMMAND [NAME...]
```

You can use the systemctl utility to provide a list of the various units currently loaded in your Linux system. A snippet example is shown in Listing 5.9.

### Listing 5.9: Looking at systemd units

```
$ systemctl list-units
UNIT                                         LOAD   ACTIVE SUB     DESCRIPTION
[...]
smartd.service                               loaded active running   Self Monitor[...]
sshd.service                                 loaded active running   OpenSSH serv[...]
sysstat.service                              loaded active exited    Resets Syste[...]
[...]
graphical.target                             loaded active active    Graphical I[...]
[...]
$
```

In Listing 5.9 you can see various units as well as additional information. Units are identified by their name and type using the format _name.type_. System services (daemons) have unit files with the _.service_ extension. Thus, the Secure Shell (SSH) daemon, sshd, has a unit filename of sshd.service.

> Many displays from the systemctl utility use the less pager by default. Thus, to exit the display, you must press the Q key. If you want to turn off the less pager for the systemctl utility, tack on the --no-pager option to the command.

Groups of services are started via *target unit files*. At system startup, the default.target unit ensures that all required and desired services are launched at system initialization. The systemctl get-default command displays the target file, as shown in Listing 5.10 on a CentOS distribution.

### Listing 5.10: Looking at the default.target link

```
$ systemctl get-default
graphical.target
$
```

Table 5.5 shows the commonly used system boot target unit files.

| Name | Description |
|---|---|
| graphical.target | Provides multiple users access to the system via local terminals and/or through the network. Graphical user interface (GUI) access is offered. |
| multi-user.target | Provides multiple users access to the system via local terminals and/or through the network. No GUI access is offered. |
| runleveln.target | Provides backward compatibility to SysVinit systems, where n is set to 1–5 for the desired SysV runlevel equivalence. |

In Table 5.5, you'll notice that systemd provides backward compatibility to the classic SysVinit systems. The SysV runlevels will be covered later in this chapter.

> The master systemd configuration file is /etc/systemd/system.conf. In this file you will find all the default configuration settings commented out via a hash mark (#). Viewing this file is a quick way to see the current systemd configuration. If you need to modify the configuration, just edit the file. However, it would be wise to peruse the file’s man page first by typing man systemd-system.conf at the command line.

