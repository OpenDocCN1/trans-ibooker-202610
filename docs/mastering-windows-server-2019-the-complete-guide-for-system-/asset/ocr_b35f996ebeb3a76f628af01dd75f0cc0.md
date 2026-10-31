```
Get-WindowsFeature -Name *telnet*
```

![](bbox=[0.13, 0.12, 0.87, 0.27])

Figure 11.33: Telnet Client is currently not installed on WEB4

By using the Add-WindowsFeature cmdlet, I should be able to make quick work of installing that feature:

```
Add-WindowsFeature Telnet-Client
```

![](bbox=[0.13, 0.4, 0.87, 0.625])

Figure 11.34: Installing Telnet Client on WEB4 via remote PowerShell

There is a ton of potential in utilizing remote PowerShell daily, not only for your servers running the full-blown Desktop Experience graphical interface but also for interacting with your security-focused Server Core deployments. Becoming familiar with working in remote PowerShell sessions will be essential to a successful deployment of Server Core in your infrastructure.