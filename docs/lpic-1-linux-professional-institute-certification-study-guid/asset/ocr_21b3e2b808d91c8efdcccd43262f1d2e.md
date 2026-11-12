+   - ext4: Also called ext4fs, it's the current version of the original Linux filesystem. It supports files up to 16 TiB, with a total filesystem size of 1 EiB. It also supports journaling and utilizes improved performance features.

+   - reiserFS: Created before the Linux ext3fs filesystem and commonly used on older Linux systems, it provides features now found in ext3fs and ext4fs. Linux has dropped support for the most recent version, reiser4fs.

+   - swap: The swap filesystem allows you to create virtual memory for your system using space on a physical drive. The system can then swap data out of normal memory into the swap space, providing a method of adding additional memory to your system. This is not intended for storing persistent data.

The default filesystem used by most Linux distributions these days is ext4fs. The ext4fs filesystem provides journaling, which is a method of tracking data not yet written to the drive in a log file, called the journal. If the system fails before the data can be written to the drive, the journal data can be recovered and stored upon the next system boot.

Many Linux administrators have taken a liking to the newer btrfs filesystem. The btrfs filesystem provides many advanced features, such as the ability to create a filesystem across multiple devices, automatic data compression, and the ability to create subvolumes.

# # Non-Linux Filesystems

One of the great features of Linux that makes it so versatile is its ability to read data stored on devices formatted for other operating systems, such as macOS and Windows. This feature makes it a breeze to share data between different systems running different operating systems.

Here's a list of the common non-Linux filesystems that Linux can handle:

+   - CIFS: The Common Internet Filesystem (CIFS) is a filesystem protocol created by Microsoft for reading and writing data across a network using a network storage device. It was released to the public for use on all operating systems.

+   - exFAT: The Extended File Allocation Table is a filesystem built on the original Microsoft FAT table architecture, and it's commonly used to format USB devices and SD cards.

+   - HFS: The Hierarchical Filesystem (HFS) was developed by Apple for its Mac systems. Linux can also interact with the more advanced HFS+ filesystem.

+   - ISO-9660: The ISO-9660 standard is used for creating filesystems on CD-ROM devices.

+   - NFS: The Network Filesystem (NFS) is an open source standard for reading and writing data across a network using a network storage device.

+   - NTFS: The New Technology Filesystem (NTFS) is the filesystem used by the Microsoft NT operating system and subsequent versions of Windows. Linux can read and write data on an NTFS partition as of kernel 2.6.x.