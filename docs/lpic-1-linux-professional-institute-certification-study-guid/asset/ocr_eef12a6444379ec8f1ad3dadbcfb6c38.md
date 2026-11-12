| Number | Start | End | Size | File system | Name | Flags |
|---|---|---|---|---|---|---|
| 1 | 1049kB | 1000MB | 999MB | fat32 | | boot, esp |
| 2 | 1000MB | 13.6GB | 12.6GB | ext4 | | |
| 3 | 13.6GB | 15.6GB | 2000MB | linux-swap(v1) | | |

(parted)

One of the selling features of the parted program is that it allows you to modify
existing partition sizes, so you can easily shrink or grow partitions on the drive. The
command prompt interface in GNU parted can be a bit cryptic at times, making it awk-
ward for first-time users to get comfortable with. However, once you become familiar
with the command structure there are lots of things you can do, making GNU parted a
versatile tool.

# Graphical Tools

Some graphical tools are also available to use if you're working from a graphical desktop
environment. The most common of these is the GNOME Partition Editor, called gparted.
Figure 3.2 shows an example of running gparted in an Ubuntu desktop environment.

# FIGURE 3.2 The GParted interface

![](bbox=[0.25, 0.53, 0.75, 0.805])

The gparted window displays each of the drives on a system one at a time, showing all
partitions contained in the drive in a graphical layout. You right-click a partition to select
options for mounting or unmounting, formatting, deleting, or resizing the partition.