
All cloud PC devices have a specific model name (contains a cloud PC) and an enrollment profile name (equates to a cloud PC provisioning profile) that can be used to filter them from an all-devices assignment:

![](img/a3e9f26159d3739e434fe676b2987c1d_142_0.png)

Figure 4.61 – Creating a device-based filter

## Publishing apps and scripted actions

We won't cover application delivery in depth in this chapter as we will do that in *Chapter 8, Application Management and Delivery*. However, I'd like to highlight the following supported formats:

- `.MSI`
- `.exe`
- `.MSIX`
- `.AppV`
- `.AppX`

If you want to provide any custom scripted actions and publish them to your cloud PC, that's possible too. You can easily publish the script action as a PowerShell script.

You can find a list of actions/scripts in this GitHub repository https://github.com/CloudManagedDesktops.

## Auto-subscribing users in the Remote Desktop client

Note that the following setting only works on Windows 11 as an endpoint and will be backported to Windows 10 soon. There’s a chance that when you read this book, the feature will already be available!

Once you have configured the ability to enroll the Remote Desktop client to your Windows 10 endpoints, you have performed the most fundamental step. However, if you stop here, the user who opens the app will be prompted to log in with their Azure AD credentials first before you see your cloud PC(s) to start the session.

This section explains how you can auto-subscribe users to the Remote Desktop (MSRDC/Store) application without the need to enter credentials!

The settings that make this way of logging on possible have to be implemented via Device | Configuration profiles, via the Settings Catalog feature. The steps are as follows:

1. First, open the Devices menu and create a new profile (for Windows 10 and later):

   ![](img/a3e9f26159d3739e434fe676b2987c1d_143_0.png)

   Figure 4.62 – Create a profile

2. Make sure that you enter a name for your configuration profile, such as Auto-subscribe Remote Desktop client, before you continue:

   ![](img/a3e9f26159d3739e434fe676b2987c1d_143_1.png)

   Figure 4.63 – Entering a name for the configuration

3. Click on + Add settings to open the catalog:

   Basics 2 Configuration settings 3 Assignments 4 Scope tags 5 Review + create

   Settings catalog

   With the settings catalog, you can choose which settings you want to configure. Click on Add settings to browse or search the catalog for the settings you want to configure.

   Learn more

   + Add settings

   Figure 4.64 – Opening the settings catalog

4. Search for Remote Desktop; a list of Remote Desktop-specific settings will appear. Click on Remote Desktop and make sure to check the box next to Auto-subscription (User):

   Settings picker

   Use commas "," among search terms to lookup settings by their keywords

   Remote desktop

   Search

   + Add filter

   Browse by category

   - RD Connection Broker
   - RD Gateway
   - RD Licensing
   - Remote Desktop
   - Remote Desktop Connection Client
   - Remote Session Environment
   - RemoteApp and Desktop Connections
   - RemoteFX for Windows Server 2008 R2

   1 results in the "Remote Desktop" category

   Select all these settings

   Setting name

   Auto-subscription (User)

   Figure 4.65 – Checking the Auto-subscription (User) box

5. Click on +Add and enter the following URL: https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery:

   Home > Devices >

   Create device configuration profile

   Windows 10 and later - Settings catalog (preview)

   Basics 2 Configuration settings 3 Assignments 4 Scope tags 5 Review + create

   + Add settings

   Remote Desktop

   Remove category

   Auto-subscription (User)

   + Add Delete Sort

   https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery

   Figure 4.66 – Entering the URL

6. Configure the right assignment settings for your use case, for example, based on Azure AD Groups, and conclude the configuration so that you can start enrolling these settings to your users/endpoint devices.

7. In the Devices menu, you can find the created device configuration profile and the status of the enrollment:

   Home > Devices >

   Auto-subscribe Remote Desktop client

   Device configuration profile - Settings Catalog

   Delete

   Device status

   Succeeded 8 Error 0 Conflict 0 Not applicable 0

   View report

   Figure 4.67 – Enrollment status

8. After the enrollment, you can click on the Remote Desktop client. You will no longer be asked to log in with your Azure AD credentials, though you may need to do this for MFA via Azure MFA (if it's been enforced):

   ![](img/a3e9f26159d3739e434fe676b2987c1d_146_0.png)

   Figure 4.68 – No login required

In the next section, you will learn how to simplify the enrollment process even further.

## Autopilot and cloud PCs – lightweight thin client (Kiosk)

The great thing about the combined capabilities of Microsoft Endpoint Manager, Autopilot, and Windows 365 is that you can configure both physical and virtual MEM-managed endpoints.

For example, within Autopilot, you can configure a multi-app Kiosk type of Surface Go, a thin client that only populates the Remote Desktop client. Full configuration and enrollment happens without user interaction and when the client is done, the end user only has to log in with their Azure AD credentials to get access to their cloud PC!

Within the Device configuration profile that you attached, for example, to your lightweight thin client, you can configure the auto-launching capability of the Remote Desktop Win32 or Store application to make the experience more awesome:

![](img/a3e9f26159d3739e434fe676b2987c1d_147_0.png)

Figure 4.69 – Configuring Kiosk mode profiles

However, that's not all we can do.

When combining this setting with the settings explained in the previous section, Auto-subscribing users in the Remote Desktop client, device configuration profile users will automatically log into the Remote Desktop client and directly log into their cloud PCs! That's pretty awesome:

![](img/a3e9f26159d3739e434fe676b2987c1d_147_1.png)

Figure 4.70 – Logging on to cloud PC directly

Now, let's cover monitoring and analytics.

## Monitoring and analytics

Ensuring that the performance and quality level of your cloud PC environment is good is just as (or perhaps even more) important as the implementation. Users need to be happy about their cloud PC and it should not impair their productivity.

Windows 365/cloud PC seamlessly integrates with all the monitoring and analytics capabilities that you use today for your physical endpoints. This means that you can easily distinguish between whether the problem is active on the physical endpoint or within the cloud PC session.

You will learn more about monitoring in Chapter 14, Monitoring and Endpoint Analytics, where we will take a much deeper dive into the specific metrics of ensuring the performance and quality of your Windows 365/cloud PC environment both proactively and reactively!

Here's a quick preview list of the reports/dashboards that are available at the time of writing:

- Startup performance
- Proactive remediations
- Recommended software
- Application reliability
- Resource performance
- Remoting connection:

![](img/a3e9f26159d3739e434fe676b2987c1d_148_0.png)

| Sign-in phase | Avg time per sign-in (sec) | Percent of sign-ins | Avg time per phase (sec) |
|---|---|---|---|
| Remoting sign-in ⓘ | 23.76 | 100 | 23.76 |
| Core sign-in ⓘ | 226.8 | 25 | 56.7 |
| Core boot ⓘ | 78.17 | 33 | 26.06 |

Figure 4.71 – Sign-in time – logon duration, Endpoint Analytics

Wait for the session to load.

During the loading process, the helpdesk user will be asked for the type of control they wish to implement:

![](img/a3e9f26159d3739e434fe676b2987c1d_150_0.png)

Figure 4.73 – Type of control

The user will receive the prompt afterwards. With that, you are now ready to troubleshoot via Quick Assist.

## Windows 11

Windows 365 provided day 1 support for Windows 11 during its **general availability (GA)** on October 5, 2021. Customers can enable the **Trusted Platform Module (TPM)** as part of their cloud PC so that they can use the hardware requirements of Windows 11.
