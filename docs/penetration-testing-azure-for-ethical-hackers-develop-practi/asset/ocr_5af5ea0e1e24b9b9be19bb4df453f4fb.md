Here is a screenshot of the first command and its output:

![](bbox=[0.125, 0.125, 0.865, 0.475])

Figure 5.25 – Example list of the available subscriptions

Congratulations! You have successfully performed a privilege escalation from the Contributor role to the Owner role.

# Executing VM extensions

As a final note on this section, we wanted to include using VM extensions to run commands on VMs. While this is not the simplest way to run commands, it may be your only option in certain situations.

Azure VM extensions are tools used by administrators to configure VMs after they have been deployed. As a way of customizing these extensions for different uses, Microsoft allows for custom script extensions to be created, based on a schema that they have defined.

This schema includes file URIs or URLs that point to script files that allow us to specify a PowerShell file that we want to execute. As attackers, we will need a web server to host the file URIs. We would suggest either using the web hosting feature built into Azure Blob Storage or using Azure App Service for hosting the files.