Here are the steps to complete this task:

+   1. Using our previously configured PowerShell session, use the Get-AzPasswords function to perform a dump of credentials for App Service:

```
Get-AzPasswords -AutomationAccounts N -StorageAccounts N -Keys N -ACR N -CosmosDB N -Verbose | Out-GridView
```

+   2. When prompted to select an Azure subscription, select your test Azure subscription and click OK:

![](bbox=[0.115, 0.29, 0.845, 0.48])

Figure 6.50 - Select the appropriate subscription

+   3. In the resulting output, you should see credentials that were dumped from the App Service configurations.

![](bbox=[0.115, 0.57, 0.85, 0.8])

Figure 6.51 – MicroBurst App Service output