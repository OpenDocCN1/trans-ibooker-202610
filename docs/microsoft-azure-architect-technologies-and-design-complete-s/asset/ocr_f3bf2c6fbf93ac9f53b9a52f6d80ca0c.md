**17.** A. It is not possible to automatically scale up.

**18.** C. Azure VM allows registration of an assembly into the Global Assembly Cache (GAC). Azure Functions and Azure App Service run within a sandbox that doesn’t support access to the GAC.

**19.** A, C, D. A RESTful API supports cross-platform software, typically converses using JSON, and is supported by Cosmos DB. There is no relation or dependency between WCF and REST.

**20.** B, D. An Azure Web App for Containers instance is compatible with Azure files, and can consume GPU resources. There are no statistics to prove the speed at which the startup happens, and GPU resources are not available with Azure containers.

# # Chapter 2: Security and Identity

**1.** A, B, D. AAD does not provide retinal scanning capabilities. This capability would need to be developed and implemented into an application manually.

**2.** D. I love these both, neither, and none questions and so do the people who write the Azure Solutions Architect Expert exam. You must admit that they really test your knowledge and create doubt. There are numerous scenarios where data at rest is encrypted; however, in the list of possible answers, only D is correct.

**3.** C. There is no charge for RBAC, but you need to know what costs money and what does not in many scenarios. Knowing which tier of the product has which features and which are free is a common theme on the exam.

**4.** C. This is a good example of a question you might see. I would think you can eliminate option A pretty easily. But option B seems possible even though you have never read or seen any information about it. So, I am torn between C and D, because I am confident option C is something AAD Connect does. If you have some good experience and have read documentation about AAD Connect and you feel you have never read anything about option B, then go with C.

**5.** A. Although conditional access can be configured to enforce numerous vulnerable scenarios, location of login attempt is the only one here that is correct.

**6.** D. `Microsoft.Authorization/roleDefinition/read` and `Microsoft.Authorization/roleDefinition/write` can do it if combined, but only `Microsoft.Authorization/roleDefinition/*` is the correct answer.

**7.** A. ARM ensures clients like the Azure Portal, Azure PowerShell, Azure CLI, and any client consuming the REST APIs are authenticated. Managing resources on Azure depends on ARM.