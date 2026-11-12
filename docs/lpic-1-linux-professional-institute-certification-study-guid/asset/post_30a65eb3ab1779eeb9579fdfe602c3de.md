
The logical volumes create entries in the /dev/mapper directory, which represent the LVM device you can format with a filesystem and use like a normal partition.

While the initial setup of an LVM is complicated, it does provide great benefits. If you run out of space in a logical volume, just add a new disk partition to the volume.

## Using RAID Technology

Redundant Array of Inexpensive Disks (RAID) technology has changed the data storage environment for most data centers. RAID technology allows you to improve data access performance and reliability, as well as implement data redundancy for fault tolerance by combining multiple drives into one virtual drive. Several versions of RAID are commonly used:

- RAID 0: Disk striping, which spreads data across multiple disks for faster access
- RAID 1: Disk mirroring, which duplicates data across two drives
- RAID 10: Disk mirroring and striping, which provides striping for performance and mirroring for fault tolerance
- RAID 4: Disk striping with parity, which adds a parity bit stored on a separate disk so that data on a failed data disk can be recovered
- RAID 5: Disk striping with distributed parity, which adds a parity bit to the data stripe so that it appears on all disks so that any failed disk can be recovered
- RAID 6: Disk striping with double parity, which stripes both the data and the parity bit so that two failed drives can be recovered

The downside is that hardware RAID storage devices can be somewhat expensive (despite what the I stands for), and they are often impractical for most home uses. Because of that, Linux has implemented a software RAID system that can implement RAID features on any disk system.

The mdadm utility allows you to specify multiple partitions to be used in any type of RAID environment. The RAID device appears as a single device in the /dev/mapper directory, which you can then partition and format to a specific filesystem.

## Partitioning Tools

After you connect a drive to your Linux system you’ll need to create partitions on it (even if there’s only one partition). Linux provides several tools for working with raw storage devices to create partitions. This section covers the most popular partitioning tools you’ll run across in Linux.

## Working with fdisk

The most common command-line partitioning tool is the fdisk utility. The fdisk program allows you to create, view, delete, and modify partitions on any drive that uses the MBR method of indexing partitions.

To use the fdisk program, you must specify the drive device name (not the partition name) of the device you want to work with:

```
$ sudo fdisk /dev/sda
[sudo] password for rich:
Welcome to fdisk (util-linux 2.23.2).
```

Changes will remain in memory only, until you decide to write them.

Be careful before using the write command.

Command (m for help):

The fdisk program uses its own command line that allows you to submit commands to work with the drive partitions. Table 3.4 shows the common commands you have available to work with.

### TABLE 3.4 Common fdisk commands

| Command | Description |
|---|---|
| a | Toggle a bootable flag |
| b | Edit BSD disk label |
| c | Toggle the DOS compatibility flag |
| d | Delete a partition |
| g | Create a new empty GPT partition table |
| G | Create an IRIX (SGI) partition table |
| l | List known partition types |
| m | Print this menu |
| n | Add a new partition |
| o | Create a new empty DOS partition table |
| p | Print the partition table |
| q | Quit without saving changes |
| s | Create a new empty Sun disk label |
| t | Change a partition's system ID |
| u | Change display/entry units |
| v | Verify the partition table |
| w | Write table to disk and exit |
| x | Extra functionality (experts only) |

The p command displays the current partition scheme on the drive:

```
Command (m for help): p

Disk /dev/sda: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000528e6

   Device Boot      Start         End      Blocks   Id  System
/Dev/sda1   *        2048    2099199    1048576   83  Linux
/Dev/sda2        2099200    20971519     9436160   83  Linux
```

Command (m for help):

In this example, the /dev/sda drive is sectioned into two partitions: sda1 and sda2. The Id and System columns refer to the type of filesystem the partition is formatted to handle. We cover that in the “Understanding Filesystems” section later. Both partitions are formatted to support a Linux filesystem. The first partition is allocated about 1 GB of space, whereas the second is allocated a little over 9 GB of space.

The fdisk command is somewhat rudimentary in that it doesn’t allow you to alter the size of an existing partition; all you can do is delete the existing partition and rebuild it from scratch.

To be able to boot the system from a partition, you must set the boot flag for the partition. You do that with the a command. The bootable partitions are indicated in the output listing with an asterisk.

If you make any changes to the drive partitions, you must exit using the w command to write the changes to the drive.

## Working with gdisk

If you're working with drives that use the GPT indexing method, you'll need to use the gdisk program:

```
$ sudo gdisk /dev/sda
[sudo] password for rich:
GPT fdisk (gdisk) version 1.0.3
```

Partition table scan:
MBR: protective
BSD: not present
APM: not present
GPT: present

Found valid GPT with protective MBR; using GPT.

Command (? for help):

The gdisk program identifies the type of formatting used on the drive. If the drive doesn't currently use the GPT method, gdisk offers you the option to convert it to a GPT drive.

> Be careful with converting the drive method specified for your drive. The method you select must be compatible with the system firmware (BIOS or UEFI). If not, your drive will not be able to boot.

The gdisk program also uses its own command prompt, allowing you to enter commands to manipulate the drive layout, as shown in Table 3.5.

### TABLE 3.5 Common gdisk commands

| Command | Description |
|---|---|
| b | Back up GPT data to a file |
| c | Change a partition's name |
| d | Delete a partition |
| i | Show detailed information on a partition |
| l | List known partition types |
| n | Add a new partition |
| o | Create a new empty GUID partition table (GPT) |
| p | Print the partition table |
| q | Quit without saving changes |
| r | Recovery and transformation options (experts only) |
| s | Sort partitions |
| t | Change a partition's type code |
| v | Verify disk |
| w | Write table to disk and exit |
| x | Extra functionality (experts only) |
| ? | Print this menu |

You'll notice that many of the gdisk commands are similar to those in the fdisk program, making it easier to switch between the two programs.

## The GNU parted Command

The GNU parted program provides yet another command-line interface for working with drive partitions:

```
$ sudo parted
GNU Parted 3.2
Using /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) print
Model: ATA VBOX HARDDISK (scsi)
Disk /dev/sda: 15.6GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
```

| Number | Start | End | Size | File system | Name | Flags |
|---|---|---|---|---|---|---|
| 1 | 1049kB | 1000MB | 999MB | fat32 | | boot, esp |
| 2 | 1000MB | 13.6GB | 12.6GB | ext4 | | |
| 3 | 13.6GB | 15.6GB | 2000MB | linux-swap(v1) | | |

(parted)

One of the selling features of the parted program is that it allows you to modify existing partition sizes, so you can easily shrink or grow partitions on the drive. The command prompt interface in GNU parted can be a bit cryptic at times, making it awkward for first-time users to get comfortable with. However, once you become familiar with the command structure there are lots of things you can do, making GNU parted a versatile tool.

## Graphical Tools

Some graphical tools are also available to use if you're working from a graphical desktop environment. The most common of these is the GNOME Partition Editor, called gparted. Figure 3.2 shows an example of running gparted in an Ubuntu desktop environment.

### FIGURE 3.2 The GParted interface

![](img/ca7617340ddbe2360d36ce3428729fa9_214_0.png)

The gparted window displays each of the drives on a system one at a time, showing all partitions contained in the drive in a graphical layout. You right-click a partition to select options for mounting or unmounting, formatting, deleting, or resizing the partition.
