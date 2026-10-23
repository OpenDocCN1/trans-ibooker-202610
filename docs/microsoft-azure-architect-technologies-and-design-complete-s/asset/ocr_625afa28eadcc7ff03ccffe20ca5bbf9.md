this VNet. Select the Service Endpoints Navigation item and click the + Add menu item button. Then select Microsoft. Sql from the Service drop-down. Select the subnet in which the Azure VM you tested from in step 2 (for example, csharp) and then click the Add button. The result should resemble Figure 5.33.

# FIGURE 5.33 Adding a service endpoint to a VNet and subnet

![](bbox=[0.15, 0.23, 0.86, 0.45])

4. Navigate to the Azure SQL database created in the Exercise 5.3 and the one you connected to in steps 1 and 2 on the Overview blade. Select Set Server Firewall at the bottom of the Firewall Settings blade and click + Add Existing Virtual Network. Enter a name and the subscription the rule will reside in. Enter the virtual network (for example, CSHARPGUITAR-VNet-B) and subnet name/address prefix (for example, csharp / 10.1.0.0/24). The configuration should resemble Figure 5.34; click the OK button.

# FIGURE 5.34 Enabling a service endpoint (VNet integration) to an Azure SQL database

![](bbox=[0.35, 0.65, 0.67, 0.93])