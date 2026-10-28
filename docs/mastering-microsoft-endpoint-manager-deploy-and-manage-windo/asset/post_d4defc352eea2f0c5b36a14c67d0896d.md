
## Compliance policies

We can define the rules and settings that users and devices must meet to be compliant. This can include actions that apply to noncompliant devices. Actions for noncompliance can alert users to the conditions of noncompliance and safeguard data on noncompliant devices.

See the following example of how you can set the risk level within Microsoft Defender when your endpoint does not meet the compliance expectations. Your device will show up as a risk in Microsoft Defender for Endpoint as well as in Intune – marked as non-compliant.

Home > Devices > Compliance policies >

### Windows 10 compliance policy

Windows 10 and later

Valid operating system builds

Configuration Manager Compliance

![](img/a3e9f26159d3739e434fe676b2987c1d_501_0.png)

Figure 13.47 – Microsoft Defender for Endpoint compliance settings

### Note

More information on compliance policies can be found in Chapter 10, Advanced Policy Management.

## Windows 365 security baselines

Windows 365 delivers its own branded set of security baselines that include different best practices that are optimized for cloud PC virtualized scenarios.

We highly recommend customers use these as they come from experience from real-world implementations. You can use these policies to lower the risk while increasing the security boundaries of your cloud PCs.

You can use security baselines to get security recommendations that can help lower risk. The Windows 365 baselines enable security configurations for Windows 10, Edge, and Microsoft Defender for Endpoint. They include versioning features and help customers choose when to update user policies to the latest release.

![](img/a3e9f26159d3739e434fe676b2987c1d_502_0.png)

Figure 13.48 – Security baselines

### Requirements for Defender for Endpoint

In the next part of this section, I'm going to explain how you configure Microsoft Defender for Endpoint via MEM to secure your virtual or physical Windows endpoints:

- 1. Go to the MEM admin center via https://endpoint.microsoft.com/#home.
- 2. Go to Endpoint security.

Figure 13.49 – Endpoint security

3. Click on Open the Microsoft Defender Security Center.

Figure 13.50 – Microsoft Defender for Endpoint

4. Click on **Next**.

![](img/a3e9f26159d3739e434fe676b2987c1d_504_0.png)

Figure 13.51 – Microsoft Defender Security Center – Step 2

5. Fill in your region, data retention policy time, and org size.

> **Note**
> You could also select the **Preview features** option to be among the first to try upcoming features.

Click **Next**.

![](img/a3e9f26159d3739e434fe676b2987c1d_505_0.png)

Figure 13.52 – Microsoft Defender Security Center – Step 3

- 6. Make sure all the settings are correct, as there is no way back. Click on **Continue**.

### Create your cloud instance

You won't be able to change some of your preferences (such as the data storage location) after clicking 'Continue'. If you want to check or make any changes, click 'Back to preferences' and review your preferences. Click 'Continue' if you want to set up your account.

![](img/a3e9f26159d3739e434fe676b2987c1d_505_1.png)

Figure 13.53 – Create your cloud instance

7. Your Microsoft Defender for Endpoint account is being created. Please wait.

![](img/a3e9f26159d3739e434fe676b2987c1d_506_0.png)

Figure 13.54 – Microsoft Defender Security Center – Step 3

Now you are ready to create the Microsoft Defender for Endpoint integration with Microsoft Intune.

### Connecting to Intune – MEM integration

Follow these steps to proceed with the integration:

- 1. Open the Security Center portal: https://securitycenter.windows.com/.
- 2. Go to Settings.

![](img/a3e9f26159d3739e434fe676b2987c1d_506_1.png)

Figure 13.55 – Settings

3. Turn the slider next to Microsoft Intune connection to On.

### Settings

![](img/a3e9f26159d3739e434fe676b2987c1d_507_0.png)

Figure 13.56 – Microsoft Intune connection

4. Click on Save preferences.

![](img/a3e9f26159d3739e434fe676b2987c1d_507_1.png)

Figure 13.57 – Preferences saved

5. At this point, Microsoft Defender integrates into MEM. You can check the status in the Endpoint security menu.

Connection status
Enabled
Last synchronized
7/16/2021, 11:10:10 AM

### MDM Compliance Policy Settings

Connect Android devices to Microsoft Defender for Endpoint

Connect iOS devices to Microsoft Defender for Endpoint

Connect Windows devices to Microsoft Defender for Endpoint

Block unsupported OS versions

### App Protection Policy Settings

Connect Android devices to Microsoft Defender for Endpoint for app protection policy evaluation

Connect iOS devices to Microsoft Defender for Endpoint for app protection policy evaluation

### Common Shared Settings

Number of days until partner is unresponsive

Figure 13.58 – Connectors and tokens – Microsoft Defender for Endpoint

> Note
> When on, compliance policies using the device threat level rule will evaluate devices, including data from this connector. When off, Intune will not use device risk details sent over this connector during device compliance calculation for policies with a device threat level configured. Existing devices that are not compliant due to risk levels obtained from this connector will also become compliant.

MDM Compliance Policy Settings: When on, compliance policies using the Device Threat Level rule will evaluate devices, including data from this connector, which is recommended to leverage the device threat level as part of the compliance policy for Windows devices.

When off, Intune will not use device risk details sent over this connector during device compliance calculations for policies with a Device Threat Level configured. Existing devices that are not compliant due to risk levels obtained from this connector will also become compliant.

### MDM Compliance Policy Settings

![](img/a3e9f26159d3739e434fe676b2987c1d_509_0.png)

Figure 13.59 – MDM Compliance Policy Settings

- 6. We now need to enroll our Windows endpoints into Defender.

![](img/a3e9f26159d3739e434fe676b2987c1d_509_1.png)

Figure 13.60 – Windows 10 devices – Microsoft Defender for Endpoint health state

- 7. Switch back to the MEM portal.

8. Go to **Endpoint security**, followed by **Endpoint detection and response.**

![](img/a3e9f26159d3739e434fe676b2987c1d_510_0.png)

Figure 13.61 – Endpoint detection and response

Before you start, download your Defender onboarding file. You can find it in Defender, under Settings | Onboarding: https://securitycenter.windows.com/preferences2/onboarding.

Figure 13.62 – Downloading the onboarding file

9. Store it somewhere on your computer and unzip the folder.

Figure 13.63 – Saving the onboarding file

- 10. Select **Windows 10 and later** and **Endpoint detection and response (MDM)** for the respective fields.

![](img/a3e9f26159d3739e434fe676b2987c1d_512_0.png)

Figure 13.64 – Endpoint detection and response (MDM) profile creation

- 11. Enter a name. Then, click **Next**.

![](img/a3e9f26159d3739e434fe676b2987c1d_512_1.png)

Figure 13.65 – Create profile 1

12. Enable the Onboarding blob setting.

### Create profile

Endpoint detection and response

Endpoint Detection and Response

Microsoft Defender for Endpoint client configuration package type

Onboarding blob

Figure 13.66 – Create profile 2

13. Select the WindowsDefenderATP.onboarding file you downloaded earlier and upload it to your tenant.

### Select file

Select file

"WindowsDefenderATP.onboarding"

select

Figure 13.67 – Uploading the onboarding file

14. Confirm that the onboarding file has been added correctly. This file includes the configuration that will be pushed to the endpoints so they know how to connect to your Defender for Endpoint tenant.

![](img/a3e9f26159d3739e434fe676b2987c1d_514_0.png)

Figure 13.68 – Creating an endpoint onboarding profile

15. Click Next.

Home > Endpoint security >

### Create profile

Endpoint detection and response

Basics  Configuration settings  Scope tags  Assignments  Review + create

Scope tags

Scope tags

Default

+ Select scope tags

Figure 13.69 – Scope tags

16. Click on Add all devices. Then, click Next.

Home > Endpoint security >

### Create profile

Endpoint detection and response

Basics  Configuration settings  Scope tags  Assignments  Review + create

Included groups

Add groups  Add all users  Add all devices

Groups

All devices

Remove

Figure 13.70 – Assignments
