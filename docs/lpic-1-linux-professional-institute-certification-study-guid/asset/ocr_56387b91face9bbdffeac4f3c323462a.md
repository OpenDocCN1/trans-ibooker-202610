Although the FHS helps standardize where files are located on Linux, not all Linux distributions follow it completely. It's best to consult with your specific Linux distribution's documentation on how it manages files within the virtual directory structure.

The FHS defines core folder names and locations that should be present on every Linux system and what type of data they should contain. Table 4.16 shows just a few of the more common folders defined in the FHS.

**TABLE 4.16 Common Linux FHS folders**

| Folder | Description |
|---|---|
| / | The root filesystem |
| /boot | Contains bootloader files used to boot the system |
| /dev | Holds device files |
| /home | Contains user data files |
| /lib | Holds shared libraries and kernel modules |
| /media | Traditionally used as a mount point for removable devices |
| /mnt | Used as the current mount point for removable devices |
| /opt | Contains data for optional third-party programs |
| /tmp | Contains temporary files created by system users |
| /usr | Contains data for standard Linux programs |
| /usr/bin | Contains local user programs and data |
| /usr/lib | Holds libraries for programming and software packages |
| /usr/local | Contains data for programs unique to the local installation |
| /usr/sbin | Contains data for system programs and data |

You can read the entire FHS standard at the Linux Foundation's reference specifications: refspecs.linuxfoundation.org.