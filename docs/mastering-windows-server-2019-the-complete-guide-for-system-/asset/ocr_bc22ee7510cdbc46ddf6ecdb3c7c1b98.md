# Joining your domain

The next logical step is, of course, joining your domain. These are the standard functions that we would perform on any new server in our environment but done in a way that you may have never encountered before since we are doing all of this strictly from the Command Prompt and PowerShell interfaces. To join a Server Core to your domain, head into PowerShell and then use the Add-Computer cmdlet. You will be asked to specify both the domain name and your credentials for joining the domain—the same information you would have to specify if you were joining a Windows Server 2019 in Desktop Experience mode to a domain. First, you must specify credentials that will be used to accomplish this domain join:

![](bbox=[0.13, 0.29, 0.87, 0.69])

Figure 10.13: Entering credentials authorized to join this system to your domain