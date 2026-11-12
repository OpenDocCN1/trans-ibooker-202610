# TABLE 3.5 Common gdisk commands (continued)

| Command | Description |
|---|---|
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

# The GNU parted Command

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