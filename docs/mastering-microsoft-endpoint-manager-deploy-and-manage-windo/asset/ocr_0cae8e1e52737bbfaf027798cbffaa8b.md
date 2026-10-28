+   - Enable ADAL for OneDrive:

```
$registryPath = "HKCU:\SOFTWARE\Microsoft\OneDrive"
$Name = "EnableADAL"
$value = "1"
IF(!(Test-Path $registryPath))
{
    New-Item -Path $registryPath -Force | Out-Null
    New-ItemProperty -Path $registryPath -Name $name -Value $value `
    -PropertyType DWORD -Force | Out-Null}
ELSE {
    New-ItemProperty -Path $registryPath -Name $name -Value $value `
    -PropertyType DWORD -Force | Out-Null}
```

These are just examples of leveraging PowerShell scripts to configure an Intune-managed Windows device. Next, we will cover compliance policies.

# Compliance policies

Microsoft Endpoint Manager can set a compliance state on a device. There are two possible outcomes for a device: compliant or noncompliant.

In Microsoft Intune, you can define the rules and settings that users and devices must meet to be compliant. If Conditional Access has been configured, then users and devices that are noncompliant can be blocked from accessing resources that contain corporate data.

> **Important Note**
> Conditional Access requires an Azure AD Premium license.

Include actions that apply to noncompliant devices. Actions for noncompliance can alert users to the conditions of noncompliance and safeguard data on noncompliant devices.