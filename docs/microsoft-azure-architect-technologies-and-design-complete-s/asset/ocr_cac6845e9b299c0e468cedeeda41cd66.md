**5.** A, C. Moving an Azure product into a different subscription can be achieved via the Azure Portal and Azure CLI. Any tool that can access the ARM can update this data. No physical move of resource is required to change the subscription or resource group. Although ASR would result in the resource likely being in a new resource group or subscription, you wouldn’t choose that tool for making the migration. There is no tool named Azure Deployment Assistance (ADA).

**6.** A, C, D. You cannot move resources into a specific Azure zone after it has provisioned; this is a role performed by the platform. At creation time you can choose how many zones you want your product placed into but cannot change it later.

**7.** A. You must have an Azure subscription to provision and access any Azure resource.

**8.** A. Both waterfall and agile approaches are organizational models and not methodologies. OOP is a coding pattern and not a deployment technique. Azure DevOps is intended to provide the ability to execute the CI/CD methodology. Azure DevOps can support the Agile model as well, but it is not its primary targeted model.

**9.** B, D. ARM templates are JSON documents and the most common means for transferring data to a REST API. ARM templates can deploy application code using the msdeploy extension when embedded into the document.

**10.** B, C. Azure Automation does not provide recommendations about how to configure Azure products. That is dependent on your requirements and not something Microsoft would attempt to drive. There are some recommendations, but not in the context of Azure Automation. There are also no security-related capabilities built into Azure Automation; that is better performed in the Security Center context.

# Chapter 9: Monitor and Recover

**1.** A. Azure Service Health is a web page that monitors the performance and availability status of all Azure products globally.

**2.** A, C, D. Activity logs are logged when clients use the REST APIs exposed by the Resource Manager. Logging into a provisioned Azure VM would use RDP or SSH, neither of which is related to Resource Manager.

**3.** A, D. KQL is the query language for running customer queries against Log Analytics; there is no GQL language. Log Analytics focuses on platform metrics and will not log an application exception generated from custom code.

**4.** B, D. ETW stands for Event Tracing for Windows and wouldn’t be something you would expect to see in application logs. The same goes for a Syslog aka system logs. Both options B and D are system/platform-focused and not application-focused. Both ETWEvents and Sys-Logs are Log Analytics metrics.

**5.** B. This is not a requirement. PowerShell scripts, graphical runbooks, PowerShell workflows, and Python scripts are all supported.