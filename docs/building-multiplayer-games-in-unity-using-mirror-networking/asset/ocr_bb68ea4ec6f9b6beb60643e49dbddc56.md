+   - **Specify identity and API access** - Here, you can specify which account we should use for the instance. Selecting a service account allows your applications on your VM to make Google API calls.
+   - **Firewall** - Here, we can choose to allow HTTP and HTTPS traffic. Enable both checkboxes.

Selecting “Management, security, disks, networking, sole tenancy” will show additional options. The one we’re interested in here is “Preemptibility.” Preemptibility is an optional selection that will limit our server to only run for up to 24 hours. Depending on how you structure your game servers, preemptibility can be a valuable option to save on costs while developing significantly. It’s similar to AWS’s spot pricing.

Finally, click the create button.

From here, you can connect to the remote server, and much like with the AWS server, it’s important to remember to enable the appropriate firewall rules within Windows!

# Azure

Azure is Microsoft’s cloud solution and competitor to AWS and GCP. Much like its competitors, Microsoft provides a free trial, with $200 worth of credit to use in your first 30 days (Figures 6-12 and 6-13).

Head over to the following URL to sign up:

```
https://azure.microsoft.com/account/free
```

Once you’ve completed the account creation and verification process, you’ll have access to the Azure portal.

Select the burger menu at the top left, and choose “Virtual Machines” from the drop-down list. Then select “Create Instance.” You should see the following diagram: