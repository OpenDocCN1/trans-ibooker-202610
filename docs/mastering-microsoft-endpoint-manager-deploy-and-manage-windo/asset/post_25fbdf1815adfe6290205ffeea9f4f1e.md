
## The Office cloud policy service

The Microsoft 365 Apps admin center (https://config.office.com) provides cloud-based policy management of Microsoft 365 enterprise apps. Policies deployed from the **Office cloud policy (OCP)** service will work on devices that are managed and unmanaged, so basically, all devices that users in your Office 365 organization are logging in to, both corporate and personally owned.

When a user signs into Microsoft 365 Apps for enterprise on a device, the policy settings roam to that device. You can also enforce some policy settings for Office web apps, iOS, macOS, and Android.

The Office cloud policy service includes many of the same user-based policy settings that are available in Group Policy or in Microsoft Intune. Conflict handling between the OCP service and policy settings from **Group Policy Object (GPO)** or Microsoft Intune is done so that Microsoft Intune or GPO will always win. A user can be a member of multiple Azure Active Directory groups with conflicting policy settings. By setting a priority on the Office policy in OCP, you can control which policy setting is applied. The highest priority is applied, with 0 being the highest priority that you can assign to a policy in the OCP service.

Requirements for using the Office cloud policy service are as follows:

- At least version 1808 of Microsoft 365 Apps for enterprise (and for the subscription versions of the Project and Visio desktop apps).
- User accounts must be created in or synchronized to **Azure Active Directory (AAD)**. The user must be signed in to Office 365 ProPlus with an AAD-based account.
- Security groups must be created in or synchronized to **AAD**, with the appropriate users added to those groups.

Pros of cloud-based policy management are as follows:

- It works on all devices – managed and unmanaged, both Microsoft Intune and SCCM.
- It works on BYOD and personal devices.
- The settings are always up to date and as it is a service from the Microsoft Office team, there is no need to download updated Administrative Template files (ADMX/ADML).

## Creating a policy configuration with the OCP service

The following are the basic steps for creating a policy configuration from the MEM admin center.

To create a policy configuration, you must be assigned one of the following roles in AAD: Global Administrator, Security Administrator, or Office Apps Admin. Intune Service Administrators do not have the right to create OCP policies.

- 1. Go to **Home | Apps | Policies for Office apps.**

You won’t have any policy configurations if this is the first time you are looking at the Office cloud policy service.

Click **Create** to create your first policy configuration.

![](img/a3e9f26159d3739e434fe676b2987c1d_428_0.png)

Figure 11.2 – Policies for Office apps

2. Enter Office policy in the Name field, then click on Select type to expand into the view you see in the following screenshot. Select This policy configuration applies to users.

When selecting this option, you are able to select a target user group from AAD. If you select This policy configuration applies to users that access documents anonymously using Office web apps, it will apply to Office online if the documents are being accessed anonymously. An IT admin can always set up external sharing so that a guest user must sign in or provide a verification code:

![](img/a3e9f26159d3739e434fe676b2987c1d_429_0.png)

Figure 11.3 – Create policy configuration – Select type

3. For the Select group option, choose Office 365 Users (a custom group).

Then click on Select group to expand into the view you see in the following screenshot. You can select AAD security groups, but a security group can only have one OCP service policy assigned:

![](img/a3e9f26159d3739e434fe676b2987c1d_430_0.png)

Figure 11.4 – Create policy configuration – Select group

- 4. If an OCP is already assigned to an AAD group, it will be grayed out in the UI as you can only assign one Office cloud policy per group in Azure AD:

![](img/a3e9f26159d3739e434fe676b2987c1d_430_1.png)

Figure 11.5 – Select the group for this policy configuration

## Configuring policies

As of this moment, there are 2,133 different policy settings within the Office cloud policy service.

Configure the policy settings to be included in the policy configuration. You can search for the policy setting name to find the policy setting that you want to configure. You can also filter on the application, on whether the policy is a recommended security baseline, and on whether the policy has been configured. The Platform column indicates whether the policy is applied to Microsoft 365 Apps for enterprise for Windows devices, Office for the web, or both.

![](img/a3e9f26159d3739e434fe676b2987c1d_431_0.png)

Figure 11.6 – Create policy configuration

In this case, we are going to configure the default file format for all Microsoft 365 enterprise apps:

- 1. Search for `Default file format`.
- 2. Select Word for **Default file format**.
- 3. Change from **Not configured** to **Enabled**.

![](img/a3e9f26159d3739e434fe676b2987c1d_432_0.png)

Do the same for the rest of the Microsoft 365 enterprise apps.

Click Create.

![](img/a3e9f26159d3739e434fe676b2987c1d_433_0.png)

Figure 11.8 – Create policy configuration

This concludes the section on Office cloud policies in Microsoft Endpoint Manager. We showed you how to create Office policies that can be deployed to all endpoints in your organization. In the next section, we will show you some tips and tricks in the OCP service.

If you enter Word, you will only see policies that are applicable to Word:

![](img/a3e9f26159d3739e434fe676b2987c1d_435_0.png)

Figure 11.10 – Filters on Application

If you select Security Baseline, you will only see policies that are applicable to Windows; there are no security baselines for other operating systems:

![](img/a3e9f26159d3739e434fe676b2987c1d_436_0.png)

Figure 11.11 – Filters on Recommendations

When you select a policy setting that is in the security baseline, you just need to select Microsoft recommended security baseline to get the policy configured to the correct value:

![](img/a3e9f26159d3739e434fe676b2987c1d_436_1.png)

Figure 11.12 – Microsoft recommended security baseline

If you check **Configured**, you will only see policies that are configured in the policy:

Figure 11.13 – Filters by Status

If you use **Configured** as a filter, you can see all the policy settings that are configured:

Figure 11.14 – Filter – Configured

> Note
> If you are only installing Microsoft Teams on your device, the Click-to-Run service is not installed and Office cloud policies are not applied to the devices.

It is possible to switch accounts in an Office application by going to File | Account | Switch account:

![](img/a3e9f26159d3739e434fe676b2987c1d_439_0.png)

Figure 11.16 – Office – Account

Microsoft Intune ADMX policies or GPO user policies can be located in the local registry,

```
HKCU\software\policies\Microsoft\Office\16.0\
```

![](img/a3e9f26159d3739e434fe676b2987c1d_439_1.png)

Figure 11.17 – Registry path for Office ADMX

Many policies from GPO or Microsoft Intune ADMX policies are set per device. That is also the case in this example and the reason for only one policy value showing up.

Policy settings retrieved from the Office cloud policy service are stored in the registry at HKEY_CURRENT_USER\Software\Policies\Microsoft\Cloud\Office\16.0. This key is overwritten each time a new set of policies is retrieved from the policy service during the check-in process.

![](img/a3e9f26159d3739e434fe676b2987c1d_440_0.png)

Figure 11.18 – Registry path for Office cloud policy

Policy service check-in activity is stored in the registry at HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Common\CloudPolicy. Deleting this key and restarting the Office apps will trigger the policy service to check in the next time an Office app such as Word or Excel is launched:

![](img/a3e9f26159d3739e434fe676b2987c1d_440_1.png)

Figure 11.19 – Registry path for Office cloud policy check-in

This concludes the section about configuring policies with the Office cloud policy service. In the next section, we will cover how Security Policy Advisor in the Apps admin center helps you improve the security settings of the policies you have configured in the Office cloud policy service.
