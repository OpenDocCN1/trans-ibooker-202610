# Windows Autopilot for existing devices

If you do not want to collect and upload the devices into Autopilot, some other options are available to you.

For devices already in Intune, which can be AAD-joined devices or co-managed devices, you can leverage Convert all targeted devices to Autopilot in the Autopilot profile:

![](bbox=[0.13, 0.24, 0.86, 0.27])

Figure 7.12 – Convert all targeted devices to Autopilot

Select Yes to register all targeted devices to Autopilot if they are not already registered. The next time registered devices go through the Windows OOBE, they will go through the assigned Autopilot scenario.

Please note that certain Autopilot scenarios require specific minimum builds of Windows. Please make sure that your device has the required minimum build to go through the scenario.

Removing this profile won’t remove affected devices from Autopilot. To remove a device from Autopilot, use the Windows Autopilot devices view.

You can also leverage Windows Autopilot for existing devices by doing traditional OS deployment from System Center Configuration Manager (SCCM) or the Microsoft Deployment Toolkit (MDT).

All that you need to do is to put the Autopilot profile in %windir%\provisioning\AutoPilot\AutopilotConfigurationFile.json.

When you have created an Autopilot profile in Microsoft Intune, you can export it by leveraging the WindowsAutopilotIntune PowerShell module:

```
Install-Module WindowsAutopilotIntune -Force
Install-Module Microsoft.Graph.Intune -Force
Connect-MSGraph
Get-Autopilotprofile
```

You will get a list of all Autopilot profiles in your tenant. The following is an example. We will leverage the ID to get it and convert it to the JSON content required:

```
@odata.type                     : #microsoft.graph.
azureADWindowsAutopilotDeploymentProfile
id                              : 264c05b0-683c-4537-87ff-
```