# ## Creating and Managing a Linux VM Using Azure CLI

+   1. Log in to the Azure Portal at https://portal.azure.com and click + Create A Resource in the top-left area of the browser. Next, click Compute and then click Virtual Machine.
+   
+   2. Select the subscription and the resource group (for example CSHARPGUITAR-SN1-RG). Enter a virtual machine name (like CSHARPGUITAR-LX) and then select the region (for example, South Central US). Select Ubuntu Server 18.04 LTS from the Image drop-down list. Click the Password radio button and enter a username and password. Select the Allow Selected Ports radio button and allow HTTP (80), HTTPS (443), and SSH (22). Click the Next: Disks button.
+   
+   3. Leave the defaults and click the Next: Networking button. Select the virtual network created in the previous chapter (I used CSHARPGUITAR-VNET-B) or create a new one. Select the subnet (mine is csharp) and click the + Next: Management button. Select the On radio button in the Auto-shutdown section, set a shutdown time, set the time zone, and if desired, enable the notification before shutdown option.
+   
+   4. Leave the remaining as the default, click the Review + Create button, and click the Create button.
+   
+   5. Once that’s complete, open a browser, and enter this internet address: shell.azure.com. Enter your credentials and then execute the following Azure CLI commands:

```
az vm get-instance-view --name <VMName> --resource-group <Name>
    --query instanceView.statuses[1] --output table

az vm stop --resource-group <Name> --name <VMName>

az vm get-instance-view --name <VMName> --resource-group <Name>
    --query instanceView.statuses[1] --output table

az vm deallocate --resource-group <Name> --name <VMName>

az vm get-instance-view --name <VMName> --resource-group <Name>
    --query instanceView.statuses[1] --output table
```

The result should be similar to that shown in Figure 4.26.