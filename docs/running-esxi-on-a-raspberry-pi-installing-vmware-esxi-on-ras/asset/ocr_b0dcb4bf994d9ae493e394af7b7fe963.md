# CHAPTER 9    NETWORK STORAGE OPTIONS FOR ESXI

+   5. Show the block devices on the system and verify that the new partition on the device is identified as *sdb1* by entering
+   
+   ```bash
+   lsblk
+   ```

![](bbox=[0.093, 0.248, 0.867, 0.508])

+   6. Install the iSCSI target framework tgt by entering
+   
+   ```bash
+   apt install tgt -y
+   ```

+   7. Verify that iSCSI has been started by entering
+   
+   ```bash
+   systemctl status tgt
+   ```

![](bbox=[0.093, 0.697, 0.863, 0.869])