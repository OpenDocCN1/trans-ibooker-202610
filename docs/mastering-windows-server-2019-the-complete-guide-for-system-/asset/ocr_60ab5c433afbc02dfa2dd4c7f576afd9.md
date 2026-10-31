# Populate the Central Store

You probably didn't have an immediate reason to check inside one of your GPOs and find out what the **Administrative Templates** folder looks like now that we enabled the Central Store, but let's go ahead and do that now. Open up GPMC and edit any of your existing GPOs. Take a look inside one of the **Administrative Templates** folders, and it's empty! You'll also notice that the name of the **Administrative Templates** folder has now changed to reflect the fact that it is looking to the Central Store for this information: **Administrative Templates: Policy definitions (ADMX files) retrieved from the central store:**

![](bbox=[0.13, 0.28, 0.87, 0.54])

Figure 5.21: The Administrative Templates folder is empty!

Give us our settings back! Thankfully, they are still sitting on the hard drive of your domain controller, in the same location where we added the new Google settings just a few minutes ago. Perform the following file copy jobs, and you should be back in business with all settings now stored inside the Central Store.

```
Copy %systemroot%\PolicyDefinitions to %systemroot%\SYSVOL\sysvol\contoso.local\Policies\PolicyDefinitions.
```