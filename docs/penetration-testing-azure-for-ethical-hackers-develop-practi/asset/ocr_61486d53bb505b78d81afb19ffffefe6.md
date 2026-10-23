18. Finally, we will need to trigger the attack by opening a Cloud Shell instance as the azureadmin user account. To do this, we will log in as the azureadmin account and open Cloud Shell in the portal. We can see in the following screenshot that our commands were executed in the Cloud Shell instance, and our contributoruser account is now an owner on the subscription:

![](bbox=[0.138, 0.215, 0.863, 0.592])

Figure 6.36 – contributoruser added as an Owner

As an attacker, we may want to be a little stealthier about our actions in this scenario. To suppress the message we see in the preceding screenshot, you can use the `| out-null` function at the end of the PowerShell command to redirect the output. Alternatively, we can use `&>/dev/null` at the end of the Bash shell attack. This will prevent the command execution from showing up in your admin's Cloud Shell output.

> **Important Note**
> 
> In a practical attack scenario, it may take a while for a victim Cloud Shell account to open a new session. To expedite this process, you could send a phishing email that links to `https://shell.azure.com/`. Not only will the link look legitimate, but it will start up a Cloud Shell session as soon as the site is loaded.