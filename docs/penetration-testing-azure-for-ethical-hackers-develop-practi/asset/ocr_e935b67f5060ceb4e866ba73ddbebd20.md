# The AzureAD module

The AzureAD module covers specific functions for managing Azure AD tenants. From a penetration-testing perspective, we will primarily be using this to enumerate information about an Azure AD tenant, but there are some privileged commands that can be used to add or modify users. Much like the Az module, this module will be required for some of the tools mentioned in the book. We will also install this module in our testing VM in the following section but will cover basic usage of the module here:

+   1. In an elevated (**Run as Administrator**) PowerShell session, run the AzureAD module installation command, as follows:

```
PS C:\> Install-Module -Name AzureAD
```

+   2. After installation, you will want to authenticate to your Azure tenant. You can do this by running the following code:

```
PS C:\> Connect-AzureAD
```

Just as with the Az module, this will prompt you to log in with an authentication window, and you can use the same PowerShell commands as you did with the Az module to help navigate the available functions.

# Azure REST APIs

The final method for accessing Azure is by using the REST APIs. APIs are one of the few ways that we can make use of stolen access tokens, and we will be seeing examples of this in attack scenarios that we will cover in later chapters. REST clients such as Postman or cURL can be used to interact with the APIs if a valid authorization token is provided. In the later examples, we will also be using the HTTPie command-line tool for interacting with the APIs.

We could potentially write an entire chapter on using the REST APIs to manage Azure, but for now, just know that these are very powerful APIs that can be used to execute many of the same actions that we would normally take with the CLI or PowerShell modules.

If you want to play around with the REST API basics, check out the tutorials on the Microsoft Azure REST API reference page, at https://docs.microsoft.com/en-us/rest/api/azure/.