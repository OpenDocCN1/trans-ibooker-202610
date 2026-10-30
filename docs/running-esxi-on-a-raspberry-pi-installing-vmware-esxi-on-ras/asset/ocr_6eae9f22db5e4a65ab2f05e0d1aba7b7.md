22. In the snapshot manager select **Second Snapshot** and click **Restore snapshot**, and click **Restore**.

![](bbox=[0.164, 0.175, 0.875, 0.329])

23. Power on the Fedora VM and verify that *NewFile002* is no longer present.

24. Power down the VM.

# Summary

In this chapter, we created Fedora, Ubuntu, and Photon OS VMs and copied the files that compose a VM to create another VM from them. We also talked about VMware Tools, the types of virtual disks that can be used with VMs, and how to take snapshots.

We used USB devices to store our VMs on. These are less than ideal, but they are usable. I monitored the USB that I was using for storage and noticed that when copying the files to create a new VM, it reached 121 degrees Fahrenheit (50 degrees Celsius); this can have an adverse effect on the performance and life of the device. In the next chapters, we will look at a few other storage options that are more appropriate.