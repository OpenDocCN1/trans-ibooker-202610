# TABLE 3.4 Common fdisk commands (continued)

| Command | Description |
|---|---|
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
/dev/sda1   *        2048    2099199    1048576   83  Linux
/dev/sda2        2099200    20971519     9436160   83  Linux
```

Command (m for help):

In this example, the /dev/sda drive is sectioned into two partitions: sda1 and sda2. The Id and System columns refer to the type of filesystem the partition is formatted to handle. We cover that in the “Understanding Filesystems” section later. Both partitions are formatted to support a Linux filesystem. The first partition is allocated about 1 GB of space, whereas the second is allocated a little over 9 GB of space.

The fdisk command is somewhat rudimentary in that it doesn’t allow you to alter the size of an existing partition; all you can do is delete the existing partition and rebuild it from scratch.

To be able to boot the system from a partition, you must set the boot flag for the partition. You do that with the a command. The bootable partitions are indicated in the output listing with an asterisk.

If you make any changes to the drive partitions, you must exit using the w command to write the changes to the drive.