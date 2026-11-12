

## 管理文件系统

After you've created a filesystem and mounted it to the virtual directory, you may have to manage and maintain it to keep things running smoothly. This section walks through some of the Linux utilities available for managing the filesystems on your Linux system.

创建文件系统并将其挂载到虚拟目录后，您可能需要对其进行管理和维护，以保持系统平稳运行。本节将介绍一些可用于管理Linux系统上文件系统的Linux实用程序。

### 检索文件系统统计信息

As you use your Linux system, there's no doubt that at some point you'll need to monitor disk performance and usage. A few different tools are available to help you do that:

使用Linux系统时，您无疑会在某个时刻需要监控磁盘性能和使用情况。有几种不同的工具可帮助您完成此操作：

- **df**: Displays disk usage by partition
- **du**: Displays disk usage by directory; good for finding users or applications that are taking up the most disk space
- **iostat**: Displays a real-time chart of disk statistics by partition
- **lsblk**: Displays current partition sizes and mount points

- **df**: 按分区显示磁盘使用情况
- **du**: 按目录显示磁盘使用情况；适用于查找占用磁盘空间最大的用户或应用程序
- **iostat**: 按分区显示磁盘统计信息的实时图表
- **lsblk**: 显示当前分区大小和挂载点

In addition to these tools, the /proc and /sys directories are special filesystems that the kernel uses for recording system statistics. Two files that can be useful when working with filesystems are the /proc/partitions and /proc/mounts files, which provide information on system partitions and mount points, respectively. Additionally, the /sys/block directory contains separate directories for each mounted drive, showing partitions and kernel-level stats.

除这些工具外，/proc和/sys目录是内核用于记录系统统计信息的特殊文件系统。在使用文件系统时可能有用的两个文件是/proc/partitions和/proc/mounts文件，它们分别提供系统分区和挂载点的信息。此外，/sys/block目录为每个已挂载驱动器包含单独的目录，显示分区和内核级统计信息。

Some filesystems, such as ext3 and ext4, allocate a specific number of inodes when created. An inode is an entry in the index table that tracks files stored on the filesystem. If the filesystem runs out of inode entries in the table, you can't create any more files, even if space is available on the drive. Using the -i option with the df command will show you the percentage of inodes used on a filesystem and can be a lifesaver.

某些文件系统，例如ext3和ext4，在创建时会分配特定数量的inode。inode是索引表中的条目，用于跟踪存储在文件系统上的文件。如果文件系统中的inode条目用完，您将无法创建更多文件，即使驱动器上还有可用空间。将-i选项与df命令一起使用将显示文件系统上已用inode的百分比，这可能是救命稻草。

### 文件系统工具

Linux uses the e2fsprogs package of tools to provide utilities for working with ext filesystems (such as ext3 and ext4). The most popular tools in the e2fsprogs package are

Linux使用e2fsprogs工具包提供用于处理ext文件系统（如ext3和ext4）的实用程序。e2fsprogs工具包中最流行的工具包括

- **blkid**: Display information about block devices, such as storage drives
- **chattr**: Change file attributes on the filesystem
- **debugfs**: Manually view and modify the filesystem structure, such as undeleting a file or extracting a corrupted file
- **dumpe2fs**: Display block and superblock group information
- **e2label**: Change the label on the filesystem
- **resize2fs**: Expand or shrink a filesystem
- **tune2fs**: Modify filesystem parameters

- **blkid**: 显示块设备信息，例如存储驱动器
- **chattr**: 更改文件系统上的文件属性
- **debugfs**: 手动查看和修改文件系统结构，例如恢复已删除的文件或提取损坏的文件
- **dumpe2fs**: 显示块和超块组信息
- **e2label**: 更改文件系统上的标签
- **resize2fs**: 扩展或缩小文件系统
- **tune2fs**: 修改文件系统参数

These tools help you fine-tune parameters on an ext filesystem, but if corruption occurs on the filesystem, you'll need the fsck program.

这些工具可帮助您微调ext文件系统上的参数，但如果文件系统发生损坏，您将需要fsck程序。

The XFS filesystem also has a set of tools available for tuning the filesystem. You'll most likely use the following:

XFS文件系统也有一套可用于调整文件系统的工具。您最可能会使用以下工具：

- xfs_admin: Display or change filesystem parameters such as the label or UUID assigned
- xfs_db: Examine and debug an XFS filesystem
- xfs_fsr: Improve the organization of mounted filesystems
- xfs_info: Display information about a mounted filesystem, including the block sizes and sector sizes, as well as label and UUID information
- xfs_repair: Repair corrupted or damaged XFS filesystems

- xfs_admin: 显示或更改文件系统参数，例如分配的标签或UUID
- xfs_db: 检查和调试XFS文件系统
- xfs_fsr: 改善已挂载文件系统的组织
- xfs_info: 显示已挂载文件系统的信息，包括块大小和扇区大小以及标签和UUID信息
- xfs_repair: 修复损坏或受损的XFS文件系统

Although these ext and XFS tools are useful, they can't help fix things if the filesystem itself has errors. For that, the fsck program is the tool to use:

```
$ sudo fsck -f /dev/sdb1
fsck from util-linux 2.31.1
e2fsck 1.44.1 (24-Mar-2018)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
/dev/sdb1: 11/655360 files (0.0% non-contiguous), 66753/2621440 blocks
$
```

The fsck program is a front end to several different programs that check the various filesystems to match the index against the actual files stored in the filesystem, such as the e2fsck program used in this example. If any discrepancies occur, run the fsck program in repair mode, and it will attempt to reconcile the discrepancies and fix the filesystem. If the fsck program is unable to repair the drive on the first run, try running it again a few times to fix any broken files and directory links. If running the fsck program multiple times doesn't repair the drive, you may have to resort to reformatting the drive and losing any data on it.

fsck程序是几种不同程序的前端，这些程序用于检查各种文件系统，以便将索引与文件系统中实际存储的文件进行匹配，例如本示例中使用的e2fsck程序。如果发生任何不一致，请以修复模式运行fsck程序，它将尝试调和不一致并修复文件系统。如果fsck程序第一次无法修复驱动器，请尝试多次运行它以修复任何损坏的文件和目录链接。如果多次运行fsck程序仍无法修复驱动器，您可能不得不重新格式化驱动器并丢失其中的所有数据。

## 总结

These days most Linux distributions do a great job of automatically detecting and installing hardware devices. However, it's a good idea to know how to find and change hardware device settings in case you ever need to modify things. The lsdev command displays the hardware settings for all devices connected to the Linux system. The lspci command displays hardware settings for PCI devices, and the lsusb command displays settings for USB devices. The kernel also populates the virtual /proc and /sys directories with real-time information about the system hardware status. The udev program monitors the system and automatically installs any kernel modules required to support hardware connected to a running system.

如今，大多数Linux发行版在自动检测和安装硬件设备方面做得很好。但是，了解如何查找和更改硬件设备设置是个好主意，以防您需要修改某些内容。lsdev命令显示连接到Linux系统的所有设备的硬件设置。lspci命令显示PCI设备的硬件设置，lsusb命令显示USB设备的设置。内核还会使用关于系统硬件状态的实时信息填充虚拟/proc和/sys目录。udev程序监控系统并自动安装支持连接到运行中系统的硬件所需的任何内核模块。

Most storage devices, such as hard disk drives and solid-state drives, allow you to partition them into subsections. Each partition works as a separate storage area that you can format with a Linux filesystem and mount in the Linux virtual directory at mount points. You can also utilize the Linux LVM technology to create virtual volumes consisting of one or more physical partitions and treat the virtual volume as a single partition. This allows you to dynamically add more disk space to an existing virtual volume to increase the storage area.

大多数存储设备，例如硬盘驱动器和固态驱动器，允许您将它们分区为子部分。每个分区都作为一个独立的存储区域，您可以使用Linux文件系统对其进行格式化，并在Linux虚拟目录的挂载点挂载它。您还可以利用Linux LVM技术创建由一个或多个物理分区组成的虚拟卷，并将虚拟卷视为单个分区。这允许您动态地向现有虚拟卷添加更多磁盘空间以增加存储区域。

Linux provides a wealth of command-line tools for partitioning, formatting, and mounting partitions. The fdisk, gdisk, and parted tools provide a menu-driven method for creating, deleting, and modifying partitions on storage devices. After you've created a partition, you must format it using the mkfs tool. When you've formatted the partition with a Linux filesystem, you can mount it into the virtual directory using the mount command.

Linux提供了丰富的命令行工具用于分区、格式化和挂载分区。fdisk、gdisk和parted工具提供了一种基于菜单的方法，用于在存储设备上创建、删除和修改分区。创建分区后，必须使用mkfs工具对其进行格式化。使用Linux文件系统格式化分区后，可以使用mount命令将其挂载到虚拟目录。

Linux also provides several tools for managing and repairing filesystems. The df and du commands are common tools for displaying available storage space on the system, and the fsck command is vital for repairing broken filesystems.

Linux还提供了多种用于管理和修复文件系统的工具。df和du命令是显示系统可用存储空间的常用工具，而fsck命令对于修复损坏的文件系统至关重要。

## 考试要点

**Describe how the BIOS and UEFI work on Linux systems.** The BIOS and UEFI are firmware embedded into computers to start the boot process. They check basic hardware components of the computer and launch the boot loader program to start the boot process.

**描述BIOS和UEFI在Linux系统上的工作原理。** BIOS和UEFI是嵌入式计算机固件，用于启动引导过程。它们检查计算机的基本硬件组件并启动引导加载程序以启动引导过程。

**Explain how to determine the hardware settings on a Linux system.** Linux provides several command-line tools for examining the hardware settings on the system, including lsdev, which displays information on all devices; lspci, which displays information on PCI devices; and lsusb, which displays information on USB devices. In addition to these tools, Linux provides the /proc and /sys virtual directories. The Linux kernel populates files in these directories in real time to display information and statistics on hardware devices.

**解释如何确定Linux系统上的硬件设置。** Linux提供了多种命令行工具来检查系统上的硬件设置，包括lsdev（显示所有设备的信息）、lspci（显示PCI设备的信息）和lsusb（显示USB设备的信息）。除了这些工具外，Linux还提供了/proc和/sys虚拟目录。Linux内核会实时填充这些目录中的文件，以显示硬件设备的信息和统计数据。

**Describe common disk types and their features.** Most legacy systems used hard disk drives (HDD) for data storage, but more modern computers use the solid-state drive (SSD) technology. Both types connect to the motherboard using three types of standard interfaces. The legacy PATA interface provides the basic slow-speed connection and is typically found in legacy systems. More modern computer systems now use the SATA interface, which is faster and more easily configured. The SCSI interface allows you to connect more disks together in a single interface and is often faster than SATA interfaces. However, it is more expensive and is usually only found on higher-end computers.

**描述常见磁盘类型及其特性。** 大多数旧系统使用硬盘驱动器（HDD）进行数据存储，但更现代的计算机使用固态驱动器（SSD）技术。两种类型都使用三种标准接口中的一种连接到主板。旧式PATA接口提供基本的低速连接，通常出现在旧系统中。更现代的计算机系统现在使用SATA接口，它更快且更易于配置。SCSI接口允许您在单个接口中连接更多磁盘，通常比SATA接口更快。但是，它更昂贵，通常只出现在高端计算机上。

**Explain the steps necessary to add a new hard drive to a Linux system.** First you must partition the hard drive to create one or more partitions. Each partition contains a single filesystem, which Linux uses to manage the files and directories stored on the storage device. The fdisk, gdisk, and parted utilities are available in Linux for partitioning a storage device. After you partition the storage device, you must format each partition using one type of filesystem. The most common filesystem currently used in Linux is ext4, but the btrfs filesystem is gaining in popularity due to some advanced features it implements. To create the filesystem, you must use the mkfs command. After creating the filesystem, you can mount the partition into the virtual directory using the mount command. If you want Linux to mount the partition automatically at boot time, add the partition to the /etc/fstab file.

**解释在Linux系统上添加新硬盘所需的步骤。** 首先必须对硬盘进行分区以创建一个或多个分区。每个分区包含一个单一文件系统，Linux使用它来管理存储设备上存储的文件和目录。Linux中提供了fdisk、gdisk和parted实用程序用于对存储设备进行分区。对存储设备分区后，必须使用一种文件系统格式化每个分区。目前Linux中最常用的文件系统是ext4，但由于btrfs文件系统实现了一些高级功能，其受欢迎程度正在提高。要创建文件系统，必须使用mkfs命令。创建文件系统后，可以使用mount命令将分区挂载到虚拟目录。如果希望Linux在引导时自动挂载分区，请将分区添加到/etc/fstab文件中。

**Explain the tools available in Linux for managing and maintaining filesystems.** Linux provides several command-line tools for displaying the status of a filesystem. The df command displays disk usage by partition, and the du command displays disk usage by directory. If an error occurs on the partition, use the fsck utility to fix the partition.

**解释Linux中用于管理和维护文件系统的可用工具。** Linux提供了多种命令行工具来显示文件系统的状态。df命令按分区显示磁盘使用情况，du命令按目录显示磁盘使用情况。如果分区发生错误，请使用fsck实用程序修复分区。

