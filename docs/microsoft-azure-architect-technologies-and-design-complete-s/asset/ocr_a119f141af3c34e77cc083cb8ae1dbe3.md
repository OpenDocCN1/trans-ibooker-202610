to another. Exercise 8.4 was straightforward and required only four steps; it was not a complicated activity. In Exercise 9.4, you will use the Site Recovery feature within the Recovery Services vault to recover an Azure VM running in West Europe (AM2) to North Europe (DB3). You will see the navigation menu item named Site Recovery in both Figure 9.28 and Figure 9.29 provided earlier.

# EXERCISE 9.4

# Recovering an Azure VM Using Azure Site Recovery

+   1. Log in to the Azure Portal at portal.azure.com.

+   2. Navigate to your Recovery Services vault that you created in Exercise 9.3 and click the Site Recovery navigation menu item. Figure 9.31 shows how the page will look. Click Step 1: Replicate Application.

# FIGURE 9.31 Azure Site Recovery, Replicate Application

![](bbox=[0.23, 0.41, 0.76, 0.62])

+   3. Select Azure from the Source drop-down list, select (Europe) West Europe from the Source Location drop-down list, select Resource Manager from the Azure Virtual Machine Deployment Model drop-down list, select the Source subscription, select the source resource group where the Azure VM you want to recover exists, and click the OK button.

+   4. Check the check box next to the VM or group of VMs you want to recover. Click the OK button, select (Europe) North Europe from the Target location drop-down list, click the Create Target Resources button, and click the Enable replication button. The blade looks the same as from Exercise 8.4’s Figure 8.19.

+   5. While the Site Recovery configuration is being deployed, take a look back at Figure 9.31. Notice Step 2, which is about managing recover plans. Once the