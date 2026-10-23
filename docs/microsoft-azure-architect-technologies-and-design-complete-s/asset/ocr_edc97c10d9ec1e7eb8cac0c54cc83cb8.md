# FIGURE 6.2 Azure PowerShell output that renders a targeted list of Azure policies

```
PS C:\> (Get-AzPolicyDefinition).Properties | Where-Object {$_.metadata.category -eq "Regulatory Compliance"} | Format-Table displayName
displayName
Microsoft Managed Control 1599 - Developer Configuration Management | Software / Firmware Integrity verification
Microsoft Managed Control 1375 - Incident Response Assistance | Automation Support For Availability Of Information / Support
Microsoft Managed Control 1605 - Developer Security Testing And Evaluation | Static Code Analysis
Microsoft Managed Control 1142 - Security Assessment And Authorization Policy And Procedures
Microsoft Managed Control 1089 - Security Training Records
Microsoft Managed Control 1285 - Telecommunications Services | Provider contingency Plan
Microsoft Managed Control 1709 - Security Function Verification
Microsoft Managed Control 1052 - Session Lock
Microsoft Managed Control 1034 - Least Privilege
Microsoft Managed Control 1623 - Boundary Protection
Microsoft Managed Control 1515 - Personnel Termination
Microsoft Managed Control 1327 - Authenticator Management | Password-based Authentication
Microsoft Managed Control 1229 - Information System Component Inventory | No Duplicate Accounting of Components
Microsoft Managed Control 1123 - Audit Review, Analysis, And Reporting | Audit Level Adjustment
Microsoft Managed Control 1474 - Emergency Power | Long-term Alternate Power Supply - Minimal Operational Capability
Microsoft Managed Control 1227 - Information System Component Inventory | Automated Unauthorized component Detection
Microsoft Managed Control 1361 - Incident Handling
Microsoft Managed Control 1594 - Developer Configuration Management
Microsoft Managed Control 1572 - Acquisition Process
Microsoft Managed Control 1331 - Authenticator Management | Password-based Authentication
Microsoft Managed Control 1132 - Protection of Audit Information | Audit Backup on Separate Physical systems / components
Microsoft Managed Control 1225 - Information System Component Inventory
Microsoft Managed Control 1640 - Transmission Confidentiality And Integrity
Microsoft Managed Control 1420 - Maintenance Personnel
Microsoft Managed Control 1658 - Secure Name / Address Resolution Service (Recursive or Caching Resolver)
```

In the next exercise, you will apply one of these Azure policies, but as you can see in Figure 6.1, there are a large number of Azure Policy definitions that you can apply at either the subscription or resource group level. The definitions represent a set of rules that include actions such as requiring that HTTPS be enabled on all app services or a limit on the number of subscription owners. The definitions are typically targeted toward fulfilling requirements for government, industry, or regional certification attainment. There are a lot of definitions and a lot of rules within them. It is up to you to know which certification you need and which template to use. Azure Blueprint should get you on the path fairly quickly. With Azure Policy you can get the actual restrictions applied fairly easily. If you need help, you can log a support case with Microsoft, and someone will help you through this. Additionally, if one of the built-in rules does not meet your requirements, guess what—you can create a custom Azure Policy and add your own group of rules to it.

In general, an Azure Policy enables the management, creation, and assignment of policy definitions. These policy definitions enforce the rules on your Azure resources. This enforcement helps you remain compliant with the service levels and standards of your company. Azure Policy also provides monitoring and scanning capabilities that identify any resource that is not in compliance with the policy. Take a look at Figure 6.3, which illustrates some of the reporting and management capabilities available to you.

The first thing I recognized as I started reviewing my Azure consumption footprint was how many resources I have provisioned. This just goes to show how quickly resources can be provisioned, and the overview of what you have can get lost. The Azure Policy Overview blade shows you the policy description. It also notes whether the resources to which it is applied are in compliance or not and the percentage of compliance for the given policy. Complete Exercise 6.2, where you will create and apply a custom policy definition.