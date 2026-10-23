**Certificates** If the automation tasks require certificate authentication, store the certificates in this secure location.

**Variables** When a runbook needs to access some value available in another executing runbook, this is how you can pass those values between them.

There is no simple way to keep all your machines in optimal shape. Teams of people and organizations exist just to support the infrastructure on which applications run. Don’t underestimate the complexities, importance, and effort required in this area. Your IT solutions need ongoing maintenance to remain reliant.

# Summary

In this chapter, you learned that Azure Migrate is the starting point to begin your movement of resources from on-premise to Azure. Consider the Azure Migrate service as well when moving resources within Azure to different regions, subscriptions, or resource groups, but don’t forget about the useful Azure Site Recovery (ASR) tool. The resources that are most commonly moved are compute resources such as Azure VMs and Azure App Services, but SQL Server on VMs need special attention for changing their location. Making a backup or an image snapshot of the VMs and their contents and then recovering them on the new instance is a valid migration and movement approach.

You learned numerous methods for migrating code to Azure compute products. As well, you learned how to continuously make updates to the application code after its initial deployment. Visual Studio, ARM templates, and Azure DevOps are a few sources for making those deployments. Azure DevOps specifically has numerous other features that support the assignment of development work, their testing, and then deployment onto the Azure compute product of choice. You now know that you shouldn’t deploy directly into production, but instead to a deployment slot or a pre-production instance.

# Exam Essentials

**Start with Azure Migrate.** Start here when you begin your journey into the cloud. This service is helpful for assessing and migrating your workloads from on-premise to the Azure platform. Azure virtual machines, databases, web applications, and data storage are the workloads that will gain the most benefit from Azure Migrate.

**Understand Azure Site Recovery (ASR).** The name defines its purpose. Its primary role is to provide the backbone of your BCDR strategy. It is useful for performing backups and replication of workloads from on-premise to Azure and between Azure regions.