4. To see a list of all the available functions in PowerZure, run the following command:

```
PS C:\> PowerZure -h
```

This is shown as follows:

![](bbox=[0.13, 0.19, 0.87, 0.54])

Figure 4.15 – Example of PowerZure's help menu

5. Part of enumerating the attack surface area is determining the actual access that a credential has and its level of access (read/write/execute). PowerZure has a function called Get-AzureTargets that we can use for this purpose. This function compares the user role to the Azure scope to make this determination. You can run the function using the following command:

```
PS C:\> Get-AzureTargets
```