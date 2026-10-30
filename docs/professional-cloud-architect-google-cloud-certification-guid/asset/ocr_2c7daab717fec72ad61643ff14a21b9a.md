# Zonal resources

Zonal resources are only accessible by other resources within the same zone. These include the following objects:

+   - **VM instances**: These reside in a particular zone.
+   - **Zonal persistent disks**: These provide persistent storage to VM instances. They can also be shared as disks between projects for the creation of snapshots and images, but not disk attachments.
+   - **Machine types**: These define the hardware configuration for your VM instances and are defined for any particular zone.
+   - **Zonal managed instance groups**: These allow you to autoscale groups of instances. The scope can be set to either regions or zones.

Now that we understand the theory, let's take a look at how to create a project. Note that if you have not done so yet, navigate to https://cloud.google.com/free to create your free-tier GCP account.

# Managing projects

To create a new project, perform the following steps:

+   1. Log in to the GCP console at https://console.cloud.google.com. Then, click on the drop-down arrow next to the name of the project you are currently logged into. A **Select a project** window will pop up. Click on **NEW PROJECT** in the upper-right corner:

![](bbox=[0.11, 0.6, 0.87, 0.87])

Figure 2.4 – Managing projects