A VM can have multiple snapshots, and you can take snapshots regardless if a VM is powered on or off. If a snapshot is taken of an active VM, it will capture the current state of its RAM as well as its disks. Due to the resources required when taking a live snapshot, I take snapshots of VMs that have been powered down. There are many nuances with snapshots that we will not be covering here, but VMware has many excellent documents available to assist and answer questions on the topic.

# Working with Snapshots

Snapshots can be initiated from the ESXi Host Client.

+   1. Log on the ESXi Host Client.
+   2. In the Navigator double-click **Virtual Machines**.
+   3. Verify that the Fedora machine is powered off.
+   4. Right-click the Fedora VM and from the **Snapshots** drop-down menu select **Take snapshot**.

![](bbox=[0.09, 0.555, 0.87, 0.805])