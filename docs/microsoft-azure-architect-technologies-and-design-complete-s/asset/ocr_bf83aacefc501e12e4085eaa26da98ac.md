Backing up your data has been covered in much detail, and the reasons to do so are very clear. Now think about backing up the source code that analyzes, creates, or manipulates that data. If you lose the code, then all you have is a big data set but nothing to do with it or learn from it. Therefore, having a backup of your code is important. Additionally, having a change log so you can see who made changes and incorporating the ability to roll back to a previous version in case of bugs or regressions are helpful features. Some years ago, a Microsoft product named Visual Source Safe (VSS) provided these kinds of capabilities. These days the Microsoft tool for such a purpose is the Repository feature within the Azure DevOps portfolio, which used to be called Team Foundation Services (TFS), aka VSTS. There will be more on this in Chapter 8. There are many other source code repositories; a popular one is GitHub, which Microsoft recently purchased. Others like Bitbucket or a local Git repository are tightly integrated with Azure and can be used to implement deployment concepts like continuous integration/continuous deployment (CI/CD) deployment flows.

# Implementing Security

You received 99% of what you need to know for the security portion of the Azure Solutions Architect Expert exam in Chapter 2. This section is more targeted toward the application itself. The content in this chapter is good to know if your goal is to become a great Azure Solutions Architect Expert. In this section, you will implement and configure Managed Identity into an Azure App Service to give your Azure App Service an identity and use that identity in creating and configuring an Azure Key Vault key secret and provide access to other Azure resources. The code that will use that identity to get a database connection string from an Azure Key Vault secret and an encryption key to encrypt and decrypt can be viewed here:

```
github.com/benperk/ASA/tree/master/Chapter07/DataEncryption
```

That code will not be discussed in detail because coding isn’t part of the exam. But note that this is the code that will be used in the next chapter. In Chapter 8, this application is used as you learn how to deploy to the Azure platform.

Source Code
You may need to perform several steps to get the code to run, such as installing the dependent NuGet packages. Additionally, you will need to update the code to point to your Azure Key Vault. If you have any questions, start an issue on GitHub.

In addition to Managed Identity and Azure Key Vault access policies, you will configure EasyAuth. EasyAuth (introduced in Chapter 2) is a quick and easy way to implement Authentication (AuthN) using Azure Active Directory within the context of Azure App Service or Azure Function.