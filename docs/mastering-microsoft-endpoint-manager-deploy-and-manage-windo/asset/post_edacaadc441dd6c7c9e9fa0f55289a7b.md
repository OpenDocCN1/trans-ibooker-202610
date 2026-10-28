
+   - **Description:** Enter a description for the profile. This setting is optional, but it is highly recommended to enter a description so that you can go back at a later point and see what the intent of this policy is:

Home > Devices > Windows >

# Create device configuration profile

Windows 10 and later - Settings catalog (preview)

1 Basics 2 Configuration settings 3 Assignments 4 Scope tags 5 Review + create

Name *

Settings Catalog Policy

Description

Platform

Windows 10 and later

Figure 10.12 – Create device configuration profile

3. In Configuration settings, select Add settings. In the settings picker, select the User Rights category or another category to see all the available settings:

Home > Devices > Windows >

# Create device configuration profile

Windows 10 and later - Settings catalog (preview)

Basics 2 Configuration settings 3 Assignments 4 Scope tags 5 Review + create

# Settings catalog

With the settings catalog, you can choose which settings you want to configure. Click on Add settings to browse or search the catalog for the settings you want to configure.

Learn more

+ Add settings

Figure 10.13 – Create device configuration profile

4. The UI will automatically expand with the settings that you have just selected – in this case, the **User Rights** policy. Before the setting catalog was released, this was a policy where IT admins were forced to leverage a custom policy in Microsoft Intune, which required a lot of specialized knowledge:

> **Pro Tip**
> 
> The tooltip always gives you the required information on what the policy does. This is a part of the metadata from the **Windows Configuration Service Providers (CSPs)**. The tooltip also includes a **Learn more** link to the Microsoft Windows docs page for the underlying CSP.

![](img/a3e9f26159d3739e434fe676b2987c1d_385_0.png)

Figure 10.14 – Settings picker

> **Pro Tip**
> If you are not configuring all settings, you can remove individual settings by clicking the *not configured* icon, and the setting will be removed when saving the settings catalog. This means that it will not affect users or devices when deployed.

5. When configuring an individual setting, you can expand it with multiple values by clicking Add:

Figure 10.15 – Create device configuration profile

6. In this case, we are allowing users and administrators the right to local log on:

Figure 10.16 – Allow Local Log On

7. You don't need any separator in the settings catalog, as you did in the past when creating the same policy as a custom policy in Microsoft Intune.

In this case, we will also deny users with the Azure AD Global Admin role or Device administrator from logging into the devices. So, I will check the security identifier (SID) in the local administrator group on my Azure AD joined devices:

![](img/a3e9f26159d3739e434fe676b2987c1d_387_0.png)

Figure 10.17 – Deny Local Log On

8. From Windows 10 version 2004, you can leverage SID for an Azure AD group:

![](img/a3e9f26159d3739e434fe676b2987c1d_387_1.png)

Figure 10.18 – Administrators Properties

9. The SID can be found in the local administrator group on Windows 10 devices that are joined to your organization's Azure Active Directory.

10. Find the policy category you want to configure and select all these settings or select one or more individual settings. In this case, we will select Microsoft Edge Update:

Basics 2 Configuration settings 3 Assignments 4 Scope tags 5 Review + create

+ Add settings

Microsoft Edge Update

Remove category

# Applications > Microsoft Edge

Remove subcategory

1 of 5 settings in this subcategory are not configured

Allow installation Enabled

Prevent Desktop Shortcut creation upon install Enabled

"Rollback to Target version" has been set to not configured

Target version override Enabled

Target version (Device) * 90.0.818.66

Update policy override Enabled

Policy (Device) Always allow updates (recommended)

# Applications > Microsoft Edge > Microsoft Edge Beta

Remove subcategory

4 of 5 settings in this subcategory are not configured

Allow installation Disabled

"Prevent Desktop Shortcut creation upon install" has been set to not configured

Figure 10.20 – Settings in this subcategory are not configured

11. Configure the settings you want to set and remove the ones you do not want to configure on your devices.

In this case, we have configured all the settings in Applications | Microsoft Edge except for one, called Rollback to Target version, which we have removed. This means that the setting has been set to not configured. This is a different behavior than some of the other profile types in Microsoft Intune, where settings are either enabled or disabled.

12. In Applications | Microsoft Edge | Microsoft Edge Beta, we have configured one setting and removed the rest:

# Edit profile - Policy baseline

Settings catalog (preview)

1 Configuration settings 2 Review + save

+ Add settings

Microsoft Edge Update

Remove category

Applications > Microsoft Edge

Remove subcategory

1 of 5 settings in this subcategory are not configured

Allow installation Enabled

Prevent Desktop Shortcut creation upon install Enabled

Target version override Enabled

Target version (Device) * 90.0.818.66

Update policy override Enabled

Policy (Device) Always allow updates (recommended)

Applications > Microsoft Edge > Microsoft Edge Beta

Remove subcategory

4 of 5 settings in this subcategory are not configured

Allow installation Disabled

Figure 10.21 – Settings in this subcategory are not configured

13. When we, at a later point in time, edit the settings catalog policy, we will only see the settings that we configured in the first place.

Now, we can add a new subcategory and continue to build on existing policies in Microsoft Intune. In this case, we will add the Microsoft Edge Canary subcategory:

# Create profile

Administrative templates

Home > Devices > Windows >

Basics  Configuration settings  Scope tags  Assignments  Review + create

All Settings
Computer Configuration
Microsoft Edge
SmartScreen settings
User Configuration

# SmartScreen settings

Computer Configuration\Microsoft Edge\SmartScreen settings

Search to filter items..

| Setting Name | State | Setting type | Path |
| Configure Microsoft Defender SmartScr... | Not configured | Device | \Microsoft Edge\SmartScreen settings |
| Configure Microsoft Defender SmartScr... | Not configured | Device | \Microsoft Edge\SmartScreen settings |
| Configure the list of domains for which ... | Not configured | Device | \Microsoft Edge\SmartScreen settings |
| Force Microsoft Defender SmartScreen ... | Not configured | Device | \Microsoft Edge\SmartScreen settings |
| Prevent bypassing Microsoft Defender S... | Not configured | Device | \Microsoft Edge\SmartScreen settings |
| Prevent bypassing of Microsoft Defende... | Not configured | Device | \Microsoft Edge\SmartScreen settings |

Figure 10.26 – SmartScreen settings

6. For each setting, there is a description of what the policy does.

7. Most policies have options for Enabled, Disabled, and Not configured – in this case, we will enable SmartScreen:

# Configure Microsoft Defender SmartScreen

\Microsoft Edge\SmartScreen settings

This policy setting lets you configure whether to turn on Microsoft Defender SmartScreen. Microsoft Defender SmartScreen provides warning messages to help protect your users from potential phishing scams and malicious software. By default, Microsoft Defender SmartScreen is turned on.

If you enable this setting, Microsoft Defender SmartScreen is turned on.

If you disable this setting, Microsoft Defender SmartScreen is turned off.

If you don't configure this setting, users can choose whether to use Microsoft Defender SmartScreen.

This policy is available only on Windows instances that are joined to a Microsoft Active Directory domain; or on Windows 10 Pro or Enterprise instances that are enrolled for device management.

Setting type: Device

Supported on: Microsoft Windows 7 or later

Enabled  Disabled  Not configured

OK

Figure 10.27 – Configure Microsoft Defender SmartScreen
