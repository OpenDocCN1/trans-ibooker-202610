These tools help you fine-tune parameters on an ext filesystem, but if corruption occurs on the filesystem, you'll need the fsck program.

The XFS filesystem also has a set of tools available for tuning the filesystem. You'll most likely use the following:

+   - xfs_admin: Display or change filesystem parameters such as the label or UUID assigned
+   - xfs_db: Examine and debug an XFS filesystem
+   - xfs_fsr: Improve the organization of mounted filesystems
+   - xfs_info: Display information about a mounted filesystem, including the block sizes and sector sizes, as well as label and UUID information
+   - xfs_repair: Repair corrupted or damaged XFS filesystems

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

# Summary

These days most Linux distributions do a great job of automatically detecting and installing hardware devices. However, it's a good idea to know how to find and change hardware device settings in case you ever need to modify things. The lsdev command displays the hardware settings for all devices connected to the Linux system. The lspci command displays hardware settings for PCI devices, and the lsusb command displays settings for USB