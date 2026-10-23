# Azure Policy

Before you get into the text portion of learning about Azure Policy, complete Exercise 6.1 to view a list of Azure policies. This will help you get a picture of what’s available and perhaps how it can be used.

# EXERCISE 6.1

# Viewing Azure Policies Using Azure PowerShell

+   1. Open Azure PowerShell IDE and log in using the following cmdlets:

```
Connect-AzAccount
$subscription = Get-AzSubscription -SubscriptionId "<SubscriptionId>"
Set-AzContext $subscription
```

+   2. Enter the following Azure PowerShell cmdlets that result in the output shown in Figure 6.1:

```
$azurePolicy = (Get-AzPolicyDefinition).Properties.metadata
$azurePolicy | Group-Object category | Select-Object Name, Count `
| Sort-Object Count -Descending
```

# FIGURE 6.1 Azure PowerShell output that renders a list of Azure policies

![](bbox=[0.175, 0.475, 0.81, 0.665])

+   3. Execute the following Azure PowerShell cmdlet, which will list all the Azure policies that are targeted toward regulatory compliance. The output is illustrated in Figure 6.2.

```
(Get-AzPolicyDefinition).Properties `
| where-object {$_ .metadata.category -eq "Regulatory Compliance"} `
| Format-Table displayName
```