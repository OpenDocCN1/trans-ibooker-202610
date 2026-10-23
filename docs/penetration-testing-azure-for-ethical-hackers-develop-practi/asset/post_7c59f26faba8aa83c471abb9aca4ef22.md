
## PowerShell

For subscriptions large and small, it is convenient to have access to the flexibility of PowerShell for parsing data. Azure subscriptions can contain large numbers of resources, and those can be quite difficult to parse by hand in the portal.

Here are some quick notes for those that are not as familiar with PowerShell:

- PowerShell is a command shell, like `cmd.exe`.
- When you are running PowerShell or the PowerShell **Integrated Scripting Environment (ISE)**, we will refer to that as a session.
- PowerShell is also a scripting language that has deep ties with the .NET runtimes and Windows APIs.
- PowerShell modules are sets of functions that can be imported into a PowerShell session.
- PowerShell modules can be imported directly from files or installed from external sources, such as the **PowerShell Gallery (PSGallery—https://www.powershellgallery.com/)**. They can also be configured to be permanently imported for all new sessions.
- PowerShell functions, or Cmdlets, are commands that can be used in a session.
- Functions with parameters allow for tab-complete—type a dash and *Tab* (or *Ctrl + space*) to see the available parameters for the function.
- PowerShell can have pipeline-able objects that allow you to use the output from one function for the input of other functions.
- The PowerShell pipeline is very powerful, and we will make extensive use of it in our examples.

We will be using several PowerShell-based tools throughout the book, and while we will try to make commands as simple as possible, it would be a good idea to get a basic understanding of how to use PowerShell.

> **Important note**
> If you're looking for non-book PowerShell resources, Microsoft does provide some free learning resources for those learning about PowerShell. These resources are a great beginner's course and will give you a head start on the concepts covered in this book: https://docs.microsoft.com/en-us/powershell/scripting/learn/more-powershell-learning.

For starters, you will need to be able to enable PowerShell script execution on your system. Chances are good that you will have some PowerShell execution policy restrictions on your testing system. As noted in this blog post by one of this book's technical reviewers, Scott Sutherland, these execution restrictions are easily bypassed (see https://blog.netspi.com/15-ways-to-bypass-the-powershell-execution-policy/).

For the installation of PowerShell modules, we will primarily use the PowerShell Gallery packages. The PSGallery (https://www.powershellgallery.com/) is a trusted, Microsoft-managed resource for package management. This is one of the easiest ways to install modules, and the Microsoft packages required for the toolkits will all be listed in the gallery.

Since we will walk through the installation of the Azure PowerShell modules in the second chapter of this book, the installation of these modules on your current system is not immediately needed.

## The Az module

The Az PowerShell Cmdlets are functions that interact with Azure to allow for the administration of Azure services. These functions allow for listing, creating, destroying, and modifying Azure resources. While some Azure services are not supported by the functions, most services that you interact with will be supported.

1. In an elevated (Run as Administrator) PowerShell session, run the Az module installation command, as follows:

```
PS C:\> Install-Module -Name Az
```

2. After installation, you will want to authenticate to your Azure tenant. You can do this by running the following code:

```
PS C:\> Connect-AzAccount
```

3. Much like the Azure CLI, this will prompt you to log in, but this time, an Azure AD Authentication Library (ADAL) authentication window will prompt you, instead of a web browser, as illustrated in the following screenshot:

![](img/4fa338e84e0a590967a142dd798ba4e2_48_0.png)

Figure 1.14 – Az PowerShell authentication prompt

From here, any command that you will use will be based off of the Microsoft guidance for approved verbs for PowerShell functions (https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/approved-verbs-for-windows-powershell-commands).

Most of the verbs that we will use are Get or Invoke, but this would be a good opportunity to try out the PowerShell tab completion.

1. If the module is not already imported into your PowerShell session, import the Az module, as follows:

```
PS C:\> Import-Module Az
```

2. Then, type Get-Az, as follows, and pause:

```
PS C:\> Get-Az
```

3. From here, you can use the Ctrl + space shortcut to expand your options, as follows:

![](img/4fa338e84e0a590967a142dd798ba4e2_49_0.png)

Figure 1.15 – Az PowerShell module command options

4. If we do the same for a PowerShell function parameter, you will get all of the available parameters for the function, as follows:

![](img/4fa338e84e0a590967a142dd798ba4e2_49_1.png)

Figure 1.16 – Az PowerShell module parameter options

5. To see the sub-modules within the Az module, we can use the following command:

```
PS C:\> Get-Module -Name az*
```

![](img/4fa338e84e0a590967a142dd798ba4e2_49_2.png)

Figure 1.17 – Listing Az PowerShell sub-modules

6. To get all of the commands available in a module, you can use the following command:

```
PS C:\> Get-Module -Name az*
```

![](img/4fa338e84e0a590967a142dd798ba4e2_50_0.png)

Figure 1.18 - Listing Az.Accounts module commands

Many of the actions that we try to accomplish in the PowerShell cmdlets can easily be found through tab completing. When in doubt, start typing what you think the command might be, and you may be surprised with how easy it is to find the real command.

## Modules – Az versus AzureRM

If you're searching around the internet for a specific Az PowerShell function to use, you may run into functions prefixed with Azure Resource Manager (AzureRM), as compared to Az. These functions are for the now-deprecated AzureRM module. This is one of the original PowerShell modules for Azure administration, so you may run into references to it.

For most AzureRM functions, you should be able to substitute Az for AzureRM to get the equivalent Az module command (Get-AzureRmVM versus Get-AzVM).

## The AzureAD module

The AzureAD module covers specific functions for managing Azure AD tenants. From a penetration-testing perspective, we will primarily be using this to enumerate information about an Azure AD tenant, but there are some privileged commands that can be used to add or modify users. Much like the Az module, this module will be required for some of the tools mentioned in the book. We will also install this module in our testing VM in the following section but will cover basic usage of the module here:

1. In an elevated (**Run as Administrator**) PowerShell session, run the AzureAD module installation command, as follows:

```
PS C:\> Install-Module -Name AzureAD
```

2. After installation, you will want to authenticate to your Azure tenant. You can do this by running the following code:

```
PS C:\> Connect-AzureAD
```

Just as with the Az module, this will prompt you to log in with an authentication window, and you can use the same PowerShell commands as you did with the Az module to help navigate the available functions.

## Azure REST APIs

The final method for accessing Azure is by using the REST APIs. APIs are one of the few ways that we can make use of stolen access tokens, and we will be seeing examples of this in attack scenarios that we will cover in later chapters. REST clients such as Postman or cURL can be used to interact with the APIs if a valid authorization token is provided. In the later examples, we will also be using the HTTPie command-line tool for interacting with the APIs.

We could potentially write an entire chapter on using the REST APIs to manage Azure, but for now, just know that these are very powerful APIs that can be used to execute many of the same actions that we would normally take with the CLI or PowerShell modules.

If you want to play around with the REST API basics, check out the tutorials on the Microsoft Azure REST API reference page, at https://docs.microsoft.com/en-us/rest/api/azure/.
