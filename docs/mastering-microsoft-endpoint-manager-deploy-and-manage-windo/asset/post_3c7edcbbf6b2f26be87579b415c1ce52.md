
Your end user can now send feedback in the Feedback Hub with their AAD account and other members of your organization can see this feedback.

Select the Feedback section from the side menu.

Under the Filter dropdown, select My organization. This will show all the feedback from users in your organization who are also signed in to the Feedback Hub using their registered AAD accounts:

![](img/a3e9f26159d3739e434fe676b2987c1d_234_0.png)

Figure 6.41 – Windows Feedback Hub

## Updating Microsoft 365 apps

When you are starting to leverage Windows Update for Business either on a cloud-managed device or a co-managed device, you should also be looking at how you are managing updating Microsoft 365 apps.

There are two options:

- Servicing profile for Microsoft 365 apps: You can see an overview of the devices in the Servicing Profile section of the Microsoft 365 apps admin center (config.office.com), including details on the next build rollout, projected waves, and information on any device failures and issues.

From there, you can click on Devices, Actions, Issues, and Settings to get more information on devices managed by the servicing profile:

![](img/a3e9f26159d3739e434fe676b2987c1d_235_0.png)

Figure 6.42 – Microsoft 365 apps servicing policy

- Administrative templates to configure policies: Administrative templates are the way that you are used to configuring policy settings in your on-premises environment with group policies.

The recommendation is to look at the new servicing capabilities provided by the Office 365 Apps admin center. We will cover this in depth in Chapter 8, Application Management and Delivery, later in this book.

This has brought us to the end of the chapter.

## Summary

In the chapter, you've learned about all the subjects you need to know in order to start deploying and updating Windows with Microsoft Endpoint Manager and Windows Update for Business for different endpoint scenarios.

We went through the different options on how to update Windows and what policy settings you should apply. If you are used to leveraging SCCM for deploying Windows Update, you probably already have some kind of ring deployment when you are deploying Windows Updates in your business. With Windows Update for Business, you can start taking the same approach as Windows Update for Business has ring-based deployments as well.

If you are not already running Windows Insider for Business, we explained why it is a good idea to start, and now you are ready to configure Windows Insider for Business in your organization. In the next chapter, we're going to take a deeper dive into the world of Windows Autopilot.

## Questions

- 1. What is Windows Update for Business?
  A. A way to update Microsoft apps
  B. A way to update Microsoft Edge
  C. A way to update Windows

- 2. What is the maximum number of days for Set feature update uninstall period?
  A. 30
  B. 60
  C. 90

- 3. What is Windows Insider for Business?
  A. A way to get pre-releases of Windows
  B. Windows Information Protection
  C. Microsoft 365 apps pre-release

## Answers

- 1. (C)
- 2. (B)
- 3. (A)

## Further reading

If you want to learn more about the Microsoft Endpoint Manager requirements after reading this chapter, please use one of the free online resources listed here:

- Learn about using Windows Update for Business – Azure, Microsoft Docs: https://docs.microsoft.com/en-us/mem/intune/protect/windows-update-for-business-configure
- Understanding hybrid Azure AD join and co-management: https://techcommunity.microsoft.com/t5/microsoft-endpoint-manager-blog/understanding-hybrid-azure-ad-join-and-co-management/ba-p/2221201

## 7 Manager Windows Autopilot

In this chapter, you will learn about Windows Autopilot, how it works, and why it could be beneficial for your organization to simplify the delivery process of your physical endpoints.

Windows Autopilot is a collection of technologies used to preconfigure brand-new devices and get them into a state where the end user can be productive.

Windows Autopilot is designed to easily onboard a brand-new device from anywhere. Previously, enterprises relied on **Operation System Deployment (OSD)**, but during the global Covid pandemic, where many employees were working from home, more and more enterprises were looking for alternatives to get new devices to their end users even when working from home. Windows Autopilot is a perfect fit for this scenario, where an **Original Equipment Manufacturer (OEM)** or reseller can send the device directly to the end user, who can then unbox the new device, get it up and running, and start working without any need to go into the office.

## Windows Autopilot overview

![](img/a3e9f26159d3739e434fe676b2987c1d_239_0.png)

Figure 7.1 – Autopilot process

You get started with Autopilot by getting your devices uploaded to the Autopilot service, creating and assigning an Autopilot profile, and then creating and assigning an ESP profile (this step is optional, but recommended).

After your device is shipped, the user simply has to enter their credentials before automatic enrollment starts. It is as simple as that:

![](img/a3e9f26159d3739e434fe676b2987c1d_239_1.png)

Figure 7.2 – Autopilot welcome screen

After entering the correct credentials, enrollment starts, and all your configuration items, policies, certificates, and applications are applied to your physical endpoint device.

As soon as this phase is completed, the user can see the desktop:

![](img/a3e9f26159d3739e434fe676b2987c1d_240_0.png)

Figure 7.3 – Autopilot setup device

If you use Enterprise State Roaming, you will see your desktop in a familiar way, as most pieces of your profile will come from the cloud down to the device.

By the way, Windows 11 works with Autopilot too, and the experience is completely modernized, as you can see in the following screenshot:

![](img/a3e9f26159d3739e434fe676b2987c1d_241_0.png)

Figure 7.4 – Autopilot Windows 11 welcome screen

Once you proceed and enter the AAD credentials of your user who has the Autopilot and other Microsoft Endpoint Manager profile configurations assigned to their account, the device enrollment setup process starts to kick in, as in the following example as part of Windows 11:

![](img/a3e9f26159d3739e434fe676b2987c1d_242_0.png)

Figure 7.5 – Autopilot Windows 11 device setup

In the next section, we're going to explain how you can add your devices to Windows Autopilot with the hardware ID as a unique indicator.

## Uploading the hardware ID to Windows Autopilot

The Windows Autopilot hardware hash is a 4K string retrieved from a running Windows 10 OS on the device by running `Get-WindowsAutoPilotInfo.ps1`:

```
New-Item -Type Directory -Path "C:\Temp\Autopilot"
Set-Location -Path "C:\Temp\Autopilot"
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Unrestricted
Install-Script -Name Get-WindowsAutoPilotInfo
Get-WindowsAutoPilotInfo.ps1 -OutputFile AutoPilotHWID.csv
```

Windows Autopilot device registration can be done within your organization manually, for testing, or for devices your organization already owns.

It is used to collect the hardware identity of devices (hardware hashes) and upload this information in a **Comma-Separated Values (CSV)** file to the Windows Autopilot service from the Microsoft Endpoint Manager admin center, and also to capture the hardware hash for manual registration, which requires booting the device into Windows 10. Therefore, this process is intended primarily for testing and evaluation scenarios.

The recommended way to get your brand-new devices into the Windows Autopilot service is to have your OEM or a Microsoft **Cloud Solution Provider (CSP)** partner upload the information.

For *Microsoft Surface* or *HoloLens* devices, you can open a support case with Microsoft Devices Autopilot Support: https://prod.support.services.microsoft.com/supportrequestform/0d8bf192-cab7-6d39-143d-5a17840b9f5f. You only need to provide the following information:

- AAD tenant ID
- AAD domain name
- Proof of ownership
- Device serial numbers

This process can also be used to deregister Autopilot from your tenant.

You can have a CSP partner upload your devices into Windows Autopilot with very little information compared to when you do it in Microsoft Intune yourself:

- `ProductKey only`
- `SerialNumber + OemManufacturerName + ModelName`

Let's describe each of these fields:

- `ProductKey`: This is the Windows product ID, that is, the PKID, a 13-digit number that corresponds to the Windows product key that was inserted into the firmware of the device at the time of manufacture (using OAv3).
- `SerialNumber`: This is the unique value assigned by the OEM to each device manufactured.
- `OemManufacturerName`: This is the value specified by the OEM in the **System Management Basic Input/Output System (SMBIOS)** firmware of the device, for example, *Microsoft Corporation* or LENOVO (more on that later).
