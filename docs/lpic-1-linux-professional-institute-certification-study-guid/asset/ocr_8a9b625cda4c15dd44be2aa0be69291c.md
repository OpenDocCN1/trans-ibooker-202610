# # Managing Filesystems

After you've created a filesystem and mounted it to the virtual directory, you may have to manage and maintain it to keep things running smoothly. This section walks through some of the Linux utilities available for managing the filesystems on your Linux system.

# ## Retrieving Filesystem Stats

As you use your Linux system, there's no doubt that at some point you'll need to monitor disk performance and usage. A few different tools are available to help you do that:

+   - **df**: Displays disk usage by partition
+   - **du**: Displays disk usage by directory; good for finding users or applications that are taking up the most disk space
+   - **iostat**: Displays a real-time chart of disk statistics by partition
+   - **lsblk**: Displays current partition sizes and mount points

In addition to these tools, the /proc and /sys directories are special filesystems that the kernel uses for recording system statistics. Two files that can be useful when working with filesystems are the /proc/partitions and /proc/mounts files, which provide information on system partitions and mount points, respectively. Additionally, the /sys/block directory contains separate directories for each mounted drive, showing partitions and kernel-level stats.

Some filesystems, such as ext3 and ext4, allocate a specific number of inodes when created. An inode is an entry in the index table that tracks files stored on the filesystem. If the filesystem runs out of inode entries in the table, you can't create any more files, even if space is available on the drive. Using the -i option with the df command will show you the percentage of inodes used on a filesystem and can be a lifesaver.

# ## Filesystem Tools

Linux uses the e2fsprogs package of tools to provide utilities for working with ext filesystems (such as ext3 and ext4). The most popular tools in the e2fsprogs package are

+   - **blkid**: Display information about block devices, such as storage drives
+   - **chattr**: Change file attributes on the filesystem
+   - **debugfs**: Manually view and modify the filesystem structure, such as undeleting a file or extracting a corrupted file
+   - **dumpe2fs**: Display block and superblock group information
+   - **e2label**: Change the label on the filesystem
+   - **resize2fs**: Expand or shrink a filesystem
+   - **tune2fs**: Modify filesystem parameters