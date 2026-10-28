
If there are any errors during the Windows Autopilot process, the end user has an option to see more details by clicking **view diagnostics**, as illustrated in the following screenshot:

![](img/a3e9f26159d3739e434fe676b2987c1d_611_0.png)

Figure 16.34 – Windows Autopilot failure

This will start the new **Windows Autopilot diagnostics** page for Windows 11, which is currently only available for the user-driven Windows Autopilot deployment mode.

The end user will then see clear icons that show statuses such as the following:

- **Task failed**
- **Task in progress**
- **Task unknown or incomplete**
- **Task successfully completed**

Under **Enrollment Status**, you can see all the checks that have been done, such as **Trusted Platform Module (TPM)** attestation and MDM enrollment, as illustrated in the following screenshot:

![](img/a3e9f26159d3739e434fe676b2987c1d_612_0.png)

Figure 16.35 – Windows Autopilot Enrollment Status

With the **Windows Autopilot diagnostics** overview, you can expand **General** and **Hardware** to get more information on the device. You can also dig into **Apps and policies status** to get more information about different errors in this area, as illustrated in the following screenshot:

![](img/a3e9f26159d3739e434fe676b2987c1d_613_0.png)

Figure 16.36 – Windows Autopilot diagnostics configuration information

You can see examples of different task messages for app failure in the following screenshot:

![](img/a3e9f26159d3739e434fe676b2987c1d_613_1.png)

Figure 16.37 – Windows Autopilot app failure messages

You can see in this **Windows Autopilot diagnostics** screen that there is an incomplete installation for an LOB MSI and an error on a Win32 app. Once you have the app **identifier (ID)**, you can go back to Microsoft Intune and continue troubleshooting from the app side.

In the next section, we will cover how to ensure that your devices have enabled the location service so that you can find your Windows devices if they are lost.

## Troubleshooting locating a Windows device

To get the location of a lost or stolen Windows device on a map, use the **Locate device** action in Microsoft Intune.

These are the Windows versions supported:

- Windows 10 version 20H2 (10.0.19042.789) or later
- Windows 10 version 2004 (10.0.19041.789) or later
- Windows 10 version 1909 (10.0.18363.1350) or later
- Windows 10 version 1809 (10.0.17763.1728) or later

The Windows **Location** service is off by default, as illustrated in the following screenshot:

![](img/a3e9f26159d3739e434fe676b2987c1d_614_0.png)

Figure 16.38 – Location service is off

It requires the user to turn it on if they want to leverage this feature or create a policy that turns the location service on. To do that, perform the following steps:

- 1. Create a policy in the Settings Catalog to enable the location service on your Windows devices, as illustrated in the following screenshot:

Home > Devices > Windows >
Create device configuration profile
Windows 10 and later - Settings catalog (preview)
1 Basics 2 Configuration settings 3 Assignments 4 Scope tags 5 Review + create
Name * Enable Location Service
Description
Platform Windows 10 and later

Figure 16.39 – Enable Location Service profile

- 2. Search for privacy and select Let Apps Access Location, as illustrated in the following screenshot:

Settings picker
Use commas "," among search terms to lookup settings by their keywords
privacy Search
+ Add filter
Browse by category
Administrative Templates\Windows Components\Internet Explorer\Privacy
Administrative Templates\Windows Components\Windows Media Player\User Interface
Privacy
99 results in the "Privacy" category Select all these settings
Setting name
Let Apps Access Location
Let Apps Access Location Force Allow These Apps
Let Apps Access Location Force Deny These Apps
Let Apps Access Location User in Control Of These Apps
Let Apps Access Messaging
...

Figure 16.40 – Let Apps Access Location

3. Change the setting to Force allow, as illustrated in the following screenshot:

Home > Windows >

# Create device configuration profile

Windows 10 and later - Settings catalog (preview)

Basics Configuration settings Assignments Scope tags Review + create

+ Add settings

Privacy Remove category

98 of 99 settings in this category are not configured

Let Apps Access Location Force allow.

Figure 16.41 – Force allow

4. After deploying the policy to your devices, you will see the following message: Some of these settings are hidden or managed by your organization. In this case, the Location for this device is on field is activated and the end user cannot turn it off as it is configured from Microsoft Intune as a policy. This is illustrated in the following screenshot:

Settings Home Find a setting Privacy Windows permissions General Speech Inking & typing personalization Diagnostics & feedback Activity history App permissions Location Camera

# Location

*Some of these settings are hidden or managed by your organization.

# Allow access to location on this device

If you allow access, you will enable Windows to use your device's capabilities to determine your location and Microsoft will use your location data to improve location services. People using this device will be able to choose if their apps have access to location by using the settings on this page. Denying access blocks Windows from providing location to Windows features, Microsoft Store apps, and most desktop apps.

Location for this device is on Change

# Allow apps to access your location

If you allow access, you can use the settings on this page to choose which apps can access your device's precise location and location history to enable location-based experiences such as directions and weather. If you are signed in with a Microsoft account on this device, your last known location is saved to the cloud, and shared with other devices where you are signed in with your Microsoft account. Denying access only blocks the apps listed on this page from accessing your location.

On

Figure 16.42 – Location service is on

## Troubleshooting Microsoft Edge

Microsoft has released a new Chromium-based version of Microsoft Edge. This new version provides best-in-class compatibility with extensions and websites. This also means that there are new ways to perform troubleshooting and new policies that can manage the Edge browser.

Let's start by taking a look at the policy troubleshooting tool that is built into the browser itself. By entering Edge : //policy/ in the address bar, you can access the Policies page, as illustrated in the following screenshot:

![](img/a3e9f26159d3739e434fe676b2987c1d_618_0.png)

Figure 16.45 – Edge policy

You can see all the policy settings and the policy value of each setting applied to the Edge browser.

To check whether the browser is updated to the latest version, you can enter edge : // settings/help in the address bar of the Edge browser. This will take you to the following screen:

## Summary

In this chapter, you've learned about most of the deployment errors that could occur during the configuration of MEM.

The knowledge you gain after reading this chapter will help you to troubleshoot some common errors while kicking the tires and will enable you to become the expert that you need and want to be in order to maintain production workloads in your business!

In the next chapter, we will learn about troubleshooting Windows 365.

## Questions

- 1. Is Microsoft Support included in your Microsoft Intune subscription?
  a) Yes
  b) No
- 2. Does Windows Location Services need to be enabled on Windows to work from Microsoft Intune?
  a) Yes
  b) No

## Answers

- 1. (A)
- 2. (A)

## Further reading

In case you want to learn more after reading this chapter, please use one of the following free online resources:

- *Troubleshoot app installation issues—Intune | Microsoft Docs*: https://docs.microsoft.com/en-us/troubleshoot/mem/intune/troubleshoot-app-install#:~:text=Intune%20provides%20app%20troubleshooting%20details%20based%20on%20the,Select%20user%20to%20select%20a%20user%20to%20troubleshoot
- *Troubleshooting packaging, deployment, and query of Windows apps—Win32 apps | Microsoft Docs*: https://docs.microsoft.com/en-us/windows/win32/appxpkg/troubleshooting
