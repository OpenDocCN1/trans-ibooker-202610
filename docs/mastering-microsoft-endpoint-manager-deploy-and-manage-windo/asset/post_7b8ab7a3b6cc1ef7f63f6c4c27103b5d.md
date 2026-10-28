
The BitLocker OS drive settings are as follows:

1. Select **Additional authentication at startup | Require**.

   BitLocker OS drive settings

   Additional authentication at startup

   Require

   Not configured

   Figure 7.22 – BitLocker OS drive settings

2. Select **OS drive recovery | Enable**.

3. Select **Save BitLocker recovery information to Azure Active Directory | Enable**.

4. Enable the BitLocker recovery information to be stored in AAD on the device object, so end users can retrieve it themselves on devices where they are the primary user. The AAD admin with the right privileges can always retrieve the BitLocker recovery key.

5. Select **Store recovery information in Azure Active Directory before enabling BitLocker | Require**. This will prevent users from enabling BitLocker unless the computer successfully backs up the BitLocker recovery information to AAD. Selecting **Require** will ensure that the recovery keys are successfully stored in AAD before enabling encryption:

![](img/a3e9f26159d3739e434fe676b2987c1d_270_0.png)

Figure 7.23 – BitLocker store recovery information

> **Important Note**
> 
> If testing Autopilot and BitLocker encryption on Hyper-V or any other virtual platform, we recommend you have 2–4 GB of memory assigned and 2–4 virtual CPUs.

6. For automatic BitLocker encryption to work, you need to enable **Trusted Platform Module** for the virtual device:

![](img/a3e9f26159d3739e434fe676b2987c1d_271_0.png)

When testing on a virtual device such as Hyper-V, you might experience different behavior than if you are testing on a physical device.

To start testing Autopilot in an easy way, there is already a guided scenario called cloud configuration in Microsoft Intune. This will be covered in the next section.

## Cloud configuration scenario

You can leverage deploying Windows 10 in a cloud configuration. In the Microsoft Endpoint Manager admin center, https://endpoint.microsoft.com/, choose **Troubleshooting + support | Guided scenarios (preview)**.

Optimize your Windows 10 devices for the cloud with a simple, secure, standardized configuration fit for your needs:

1. Select **Start** in the **Deploy Windows 10 in cloud configuration** guided scenario.
2. In the introduction, select **Next**.

![](img/a3e9f26159d3739e434fe676b2987c1d_272_0.png)

Figure 7.25 – Guided scenarios

### Introduction

Windows 10 in a cloud configuration helps to standardize and simplify device management for users with focused workflow needs. You can use a cloud configuration to configure new devices or to repurpose and extend the life of existing hardware. It works on any Windows 10 Pro, Windows 10 Enterprise, or Windows 10 Education device. Let's now learn more and evaluate the cloud configuration.

The following guided workflow makes it easy to deploy the recommended apps and device configurations found in the cloud configuration overview and setup guide.

### Who is this configuration useful for?

Windows 10 cloud configuration is designed for device users with simplified needs, such as productivity and browsing. Ideal candidates are the following groups of users in your organization:

- Those who use a focused set of apps curated by IT for their workflow needs, such as email, Microsoft Teams, a browser, and essential productivity and line-of-business apps. Apps can be delivered directly to the device or through virtualization.
- Those who have no dependency on an on-premises infrastructure to be successful in their role.
- Those who use devices that do not require complex setting configurations or custom agents.
- As an admin, you get to enjoy the benefits of a standardized device configuration applied across the organization, simplifying management, troubleshooting, and device replacements.
- End users enjoy a familiar Windows 10 experience optimized for the cloud with just the apps and settings they need.

### What you will need to continue

Make sure you have enabled automatic enrollment. You can manage automatic enrollment settings under **Devices | Windows | Windows enrollment | Automatic enrollment**.

### Basic

This is the step where you are configuring Windows Autopilot:

1. For **Apply device name template**, select **Yes**.
2. Enter a name for the Autopilot naming template, such as `Win-%RAND:5%`.
3. Enter a resource prefix name, such as `CloudConfig`.

### Deploy Windows 10 in cloud configuration

Introduction  **2 Basics**  3 Apps  4 Assignments  5 Review + deploy

### Autopilot device name template

Devices will be configured to enroll with Windows Autopilot. You can apply a device name template to organize your devices.

Apply device name template — No / Yes

Create a unique name for your devices. Names must be 15 characters or less, and can contain letters (a-z, A-Z), numbers (0-9), and hyphens. Names must not contain only numbers. Names cannot include a blank space. Use the `%SERIAL%` macro to add a hardware-specific serial number. Alternatively, use the `%RAND:x%` macro to add a random string of numbers, where x equals the number of digits to add.

Enter a name * — `Win-%RAND:5%`

### Resource name prefix

Give a name to the resources that will be created and deployed as part of cloud config. The list at the bottom of this page shows how the name will appear next to each resource.

Enter a resource prefix name * — `CloudConfig`

![](img/a3e9f26159d3739e434fe676b2987c1d_274_0.png)

### Resources to be created

All these profiles will have the prefix selected:

- CloudConfig M365 (Teams)
- CloudConfig Microsoft Edge
- CloudConfig security baseline
- CloudConfig Autopilot profile
- CloudConfig ESP
- CloudConfig OneDrive Known Folder Move settings
- CloudConfig Microsoft Edge app settings

### Assignments

Select **Create a new group** and for **Group name**, choose CloudConfig.

This will create an empty group so you can put devices in that group and test the deployment as necessary:

Home > Troubleshooting + support >

### Deploy Windows 10 in cloud configuration

Introduction   Basics   Apps   4 Assignments   5 Review + deploy

### Choose groups

- Create a new group
- Choose an existing group

> We recommend assigning cloud config to devices that don’t have other profiles and apps assigned to them yet. You can remove or exclude existing resources from groups temporarily and add them back in after you deploy cloud config.

Group name * — CloudConfig

Figure 7.28 – Cloud configuration assignments

### Deploying

Review your settings in the summary and select Deploy:

Home > Troubleshooting + support >

### Deploy Windows 10 in cloud configuration

✓ Introduction   ✓ Basics   ✓ Apps   ✓ Assignments   5 Review + deploy

### Summary

### Basics

Apply device name template
Yes

Enter a name
Win-%RAND:5%

Enter a resource prefix name
CloudConfig

### Apps

Selected apps
Microsoft Teams
Microsoft Edge for Windows 10
Outlook

### Assignments

Group name
CloudConfig

### What happens when I select Deploy?

The group you chose will be configured with Windows 10 in cloud configuration.

Previous   Deploy

Figure 7.29 – Cloud configuration Review + deploy

4. All the profiles will now be created:

Home > Troubleshooting + support >

### Deploy Windows 10 in cloud configuration

Deployment succeeded.

### Deployment details

| Resource | Resource Type | More | Status | Assignment |
| --- | --- | --- | --- | --- |
| CloudConfig | AAD Security Group | Docs | Created | -- |
| CloudConfig M365 (Teams) | M365 App Suite | Docs | Created | Assigned |
| CloudConfig Microsoft Edge | App | Docs | Created | Assigned |
| CloudConfig security baseline | Windows 10 security baseline | Docs | Created | Assigned |
| CloudConfig Autopilot profile | Autopilot profile | Docs | Created | Assigned |
| CloudConfig ESP 20210419_0:04:18 | Enrollment Status Page | Docs | Created | Assigned |
| CloudConfig OneDrive Known Folder Move | Administrative template | Docs | Created | Assigned |
| CloudConfig Microsoft Edge app settings | Administrative template | Docs | Created | Assigned |
| CloudConfig compliance policy | Compliance policy | Docs | Created | Assigned |
| CloudConfig built-in app removal script | Script | Docs | Created | Assigned |
| CloudConfig update ring 20210419_0:04:18 | Windows 10 update ring | Docs | Created | Assigned |

Figure 7.30 – Cloud configuration deployment succeeded

Here is what you can do next:

- Add devices to the group you configured.
- Add your preregistered Autopilot devices or other existing devices to the group you configured. For existing devices, we recommend removing other profiles and apps and resetting them, so they start fresh with just the cloud configuration applied.
- Deploy essential line-of-business apps and configurations.

We recommend keeping additional essential configurations to a minimum, including the number of line-of-business apps you deploy on top of the cloud configuration. This helps keep device management and troubleshooting simpler.
