Also notice in Figure 8.12 that there are links next to the resource group (change) and subscription (change), which will take you to the same location as if you had clicked the links in the Move menu. Those are the easy ones, because you are really only updating some data in a database. A real physical move of Azure products such as Azure virtual machines, databases, or Azure App Service apps requires more work. The work required would be similar to a migration from on-premise, excluding the step where you assess the application-specific configurations. The configurations on your on-premises installation might not be supported on the Azure location you are migrating to. That should not cause problems for your move because you are already on Azure. Let’s discuss the following in more detail:

+   - Moving Azure App Services
+   - Moving Azure Virtual Machines

Each scenario will be a little different because of the platform design for each product when it comes to moving the physical resource between regions or zones. For example, currently Azure App Service apps do not support zones, so you wouldn’t be able to do a migration like that. There is work to change this, but it is not clear if you would be ever able to pick the zone specifically or if it would be managed by the PaaS-focused platform code.

# Moving Azure App Services

Remember that Azure App Service apps run on an *App Service Plan (ASP)*, which is synonymous with a VM. You can have multiple app services on an ASP, which means you have multiple websites running on a VM. An alternative for moving an app service is a concept called cloning. Using this feature won’t move the website, but instead it will make an identical copy of it so you can then move and install the copy into another location that you specify. Once the site is moved and tested, you can remap the custom domain name to the new location and turn off the old one. If you navigate to the app service you want to relocate, you will find a link named Clone App in the navigation menu. The logic in the portal then walks you through the cloning process.

Some Azure PowerShell cmdlets result in the same outcome. Here are some example cmdlets:

```
```powershell
$srcapp = Get-AzWebApp -ResourceGroupName SrcResGrp -Name source-webapp
$destasp = New-AzAppServicePlan -Location "North Central US" `
            -ResourceGroupName DestinationAzureResourceGroup `
            -Name DestinationAppServicePlan -Tier Standard
New-AzWebApp -ResourceGroupName DestResGrp -Name destwebapp `
            -Location "South Central US" -AppServicePlan $destasp `
            -SourceWebApp $srcapp
```
```

Basically, the cmdlet retrieves the configuration details of the web app (aka app service) and stores it in the variable named $srcapp. $srcapp then is used as the source for the