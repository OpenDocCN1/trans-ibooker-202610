
Application code can be manually copied to systems by using credentials stored in the publish profile. This profile contains the following credential options:

- Web Deploy
- FTP
- ReadOnly FTP
- Zip Deploy
- Database Connection Strings

As an attacker with the Contributor role on the App Service application, we can access this publish profile to gain access to the credentials. This can be done in the overview tab for the application in the Azure portal.

![](img/4fa338e84e0a590967a142dd798ba4e2_258_0.png)

Figure 6.49 – The App Service Get publish profile link

These profiles can also be collected in PowerShell, using the Get-AzWebAppPublishingProfile function. Since we have been making good use of Get-AzPasswords in this chapter, we will continue our exercises by showing how it can be used to gather credentials for App Service applications.

## Hands-on exercise – Extracting credentials from App Service

Here is the task that we will complete in this exercise:

- Task 1: Use MicroBurst to extract App Service credentials from subscriptions

Here are the steps to complete this task:

1. Using our previously configured PowerShell session, use the Get-AzPasswords function to perform a dump of credentials for App Service:

```
Get-AzPasswords -AutomationAccounts N -StorageAccounts N -Keys N -ACR N -CosmosDB N -Verbose | Out-GridView
```

2. When prompted to select an Azure subscription, select your test Azure subscription and click OK:

![](img/4fa338e84e0a590967a142dd798ba4e2_259_0.png)

Figure 6.50 - Select the appropriate subscription

3. In the resulting output, you should see credentials that were dumped from the App Service configurations.

![](img/4fa338e84e0a590967a142dd798ba4e2_259_1.png)

Figure 6.51 – MicroBurst App Service output

Now that we have access to the app service publish profile, we will see how these credentials can be used with the application.

## Lateral movement, escalation, and persistence in App Service

After gathering credentials from an App Service publish profile, we have a few options for persisting, moving laterally, and/or escalating privileges. First things first, we will want to review the application files for any credentials that may be stored in existing configuration files or application code from previous deployments. The easiest way to do this is by connecting to the FTP server associated with the application. You can obtain the FTP server endpoint in the Azure portal (**App Service → Deployment Center → FTP Credentials**). You can also obtain the endpoint using the Azure CLI by running the following command:

```
az webapp deployment list-publishing-profiles --name <app-name> --resource-group <group-name> --query "[? ends_with(profileName, 'FTP')].{profileName: profileName, publishUrl: publishUrl}"
```

You can then connect using an FTP client. If you don't have a preferred FTP client, you can always use the Windows explorer with an FTP:// path.

![](img/4fa338e84e0a590967a142dd798ba4e2_260_0.png)

*Figure 6.52 – Sample FTP directory of an App Service application*

At this point, we can also upload a web shell that would allow persistent access to the application server, and it would allow us to run commands to generate tokens for any attached managed identities. If you go this route, make sure that you use a web shell that you trust and password-protect the web shell to prevent other attackers from getting access to your shell.

As an alternative to web shells, it is possible to run commands on the App Service server from the portal, in the Console section.

![](img/4fa338e84e0a590967a142dd798ba4e2_261_0.png)

Figure 6.53 – The App Service Console interface

This interface is convenient, but it does require you to be authenticated to Azure AD as a Contributor. So, if you lose your Contributor account, you may be out of luck.

Finally, there is an alternative management interface that can be used for managing the application. This interface can be found under the $APP_NAME.scm.azurewebsites.net subdomain, instead of the main App Service URL ($APP_NAME.azurewebsites.net).

![](img/4fa338e84e0a590967a142dd798ba4e2_261_1.png)

Figure 6.54 – The App Service Kudu interface

This interface is the Kudu (https://github.com/projectkudu/kudu) service, which can allow for a number of different attack options. This interface has CMD and PowerShell consoles, a file browser, and exports of all the environmental settings and variables.

Additionally, this interface is available without authenticating to Azure AD. By accessing the affected site on the https://$APP_NAME.scm.azurewebsites.net/basicauth page, you can just enter the credentials from the publish profile. This is really handy if you need to regain access to a subscription.

> **Important Note**
> While we have covered some of the lateral movement options in this chapter, one of the authors has previously put together a blog post that goes into more depth on some of these techniques: https://blog.netspi.com/lateral-movement-azure-app-services/.

As a final note for this section, we will mention that App Service applications frequently have managed identities attached to them. Since we previously covered escalating privileges using managed identities (Chapter 5, Exploiting Contributor Permissions on IaaS Services), we wanted to note it here as an option to look out for when attacking App Service applications. The application created for the chapter scenarios does have an attached managed identity, and while we don't have an exercise for it, you can still practice your managed identity exploitation skills with the example App Service app.

Moving on to the final section of this chapter, we will review Automation Accounts and how we can extract credentials from them.

## Extracting credentials from Automation Accounts

One of the goals associated with many cloud environments is automation. This could be as simple as automating system configuration changes and patch management, or as complex as automatically rotating SSL certificates on web applications and storing them in key vaults.

These actions may be accomplished by running code in an Automation Account in a **runbook**. These runbooks are just code blocks stored in the Automation Account. The code can be in PowerShell or Python (2 or 3), and the in-browser editor makes it really easy to integrate common Azure management functions into the code.

![](img/4fa338e84e0a590967a142dd798ba4e2_263_0.png)

Figure 6.55 – Options for runbook creation

There are a million different tasks that can be automated in Azure, but these actions often require access to a security principal in the tenant or credentials for external services. In the case of a security principal, a Run as account can be added to the Automation Account. This account can be granted permissions within the subscription, or tenant, and the runbook can use the account to take actions on its behalf.

Additionally, the Run as account can be granted access policy rights on key vaults. This would allow the account to pull any cleartext credentials that it may need from a key vault, versus storing the credentials in the automation account, or as variables in the runbook code itself.

> **Important Note**
> 
> Due to the frequent usage of Automation accounts to access key vaults in environments, a function was added to the MicroBurst toolkit to automate the extraction of Key Vault values using Automation account runbooks. Take a look at this NetSPI blog to get a better understanding of the extraction process and how it resulted in the discovery of the CVE-2019-0962 vulnerability:
> 
> https://www.netspi.com/blog/technical/cloud-penetration-testing/azure-automation-accounts-key-stores/.

To that point, cleartext credentials can be stored directly in Automation Account runbooks as variables in the code (not a best practice), or as credential objects in the Automation Accounts. These credential objects can be retrieved by cmdlets in the runbook code and cast to variables that can be passed to functions for authentication. It's important to note that anyone with the rights to modify and run runbooks in the Automation Account will have the ability to retrieve these credentials.

Here are some practical examples of stored credentials that the authors have seen in Automation Accounts during Azure penetration tests:

- Stored local administrator credentials for virtual machines
- Stored credentials for Global Administrator privileged accounts
- Domain join credentials hardcoded into the runbook code

While we won't cover how to read runbook code to find credentials, we will show how you can use functions in MicroBurst to extract any stored credentials and authentication certificates for the attached Run as accounts for Automation Accounts.

## Automation Account credential extraction overview

At a high level, here is how we will extract the stored Automation Account credentials and Run as account certificates from an Automation Account:

1. First, we will need to create a new runbook in the account. Ideally, we would name this something generic, such as `AzureAutomationTutorials`, to blend in.
2. This runbook will cast the Cred-1 credential item to a variable, then output the username and password to the job output:
   
   ```powershell
   $myCredential = Get-AutomationPSCredential -Name 'Cred-1'
   $userName = $myCredential.UserName
   $password = $myCredential.GetNetworkCredential().Password
   $userName
   $password
   ```
3. We will follow a similar process for exporting the attached Automation account Run as certificates:

```powershell
$RunAsCert = Get-AutomationCertificate -Name 'AzureRunAsCertificate'
$CertificatePath = Join-Path $env:temp RunAsCertificate.pfx
```
