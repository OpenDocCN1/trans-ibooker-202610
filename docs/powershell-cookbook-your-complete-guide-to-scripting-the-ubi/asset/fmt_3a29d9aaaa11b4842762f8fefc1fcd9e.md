
# Get-MachineStartupShutdownScript

From *PowerShell Cookbook* (O'Reilly) by Lee Holmes (http://www.leeholmes.com/guide)

## Synopsis

Get the startup or shutdown scripts assigned to a machine.

## Example

```powershell
PS > Get-MachineStartupShutdownScript -ScriptType Startup
```
Gets startup scripts for the machine.

## Parameters

```powershell
param(
    ## The type of script to search for: Startup, or Shutdown.
    [Parameter(Mandatory = $true)]
    [ValidateSet("Startup","Shutdown")]
    $ScriptType
)
```

## Script

```powershell
Set-StrictMode -Version 3

## Store the location of the group policy scripts for the machine
$registryKey = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\System\Scripts"

## There may be no scripts defined
if(-not (Test-Path $registryKey))
{
    return
}

## Go through each of the policies in the specified key
foreach($policy in Get-ChildItem $registryKey\$scriptType)
{
    ## For each of the scripts in that policy, get its script name
    ## and parameters
    foreach($script in Get-ChildItem $policy.PsPath)
    {
        Get-ItemProperty $script.PsPath | Select-Object Script,Parameters
    }
}
```

For more information about working with the Windows Registry in PowerShell, see [Chapter 21](#ch21.xhtml#the_windows_registry). For more information about running scripts, see [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools).

## See Also

- [Recipe 1.2, "Run Programs, Scripts, and Existing Tools"](#ch01.xhtml#run_programs_scripts_and_existing_tools)
- [Chapter 21](#ch21.xhtml#the_windows_registry)

# 27.6 Deploy PowerShell-Based Logon Scripts

## Problem

You want to use a PowerShell script in a logon, logoff, startup, or shutdown script.

## Solution

Simply add a new script in the PowerShell Scripts tab.

## Discussion

In the first version of PowerShell, launching a PowerShell script as a Group Policy script was a difficult task. Although you could use the `-Command` parameter of `powershell.exe` to invoke a command, the quoting rules made it difficult to specify the script correctly. After getting the quoting rules correct, you still had to contend with the Execution Policy of the client computer.

Fortunately, the situation improved significantly. Group Policy now supports PowerShell scripts as first-class citizens for the four different user and computer scripts.

When Group Policy's native support isn't an option, `powershell.exe` includes two parameters that make it easier to control the execution environment: `-ExecutionPolicy` and `-File`. For more information about these (and PowerShell's other) parameters, see [Recipe 1.17](#ch01.xhtml#invoke_a_powershell_script_from_outside_).

## See Also

- [Recipe 1.17, "Invoke a PowerShell Command or Script from Outside PowerShell"](#ch01.xhtml#invoke_a_powershell_script_from_outside_)

# 27.7 Enable or Disable the Windows Firewall

## Problem

You want to enable or disable the Windows Firewall.

## Solution

To manage the Windows Firewall, use the `LocalPolicy.CurrentProfile.FirewallEnabled` property of the `HNetCfg.FwMgr` COM object:

```powershell
PS > $firewall = New-Object -com HNetCfg.FwMgr
PS > $firewall.LocalPolicy.CurrentProfile.FirewallEnabled = $true
PS > $firewall.LocalPolicy.CurrentProfile.FirewallEnabled
True
```

## Discussion

The `HNetCfg.FwMgr` COM object provides programmatic access to the Windows Firewall in Windows XP SP2 and later. The `LocalPolicy.CurrentProfile` property provides the majority of its functionality.

For more information about managing the Windows Firewall through its COM API, visit [the Microsoft documentation site](http://msdn.microsoft.com) and search for "Using Windows Firewall API." The documentation provides examples in VBScript but gives a useful overview of the functionality available.

For more information about working with COM objects in PowerShell, see [Recipe 17.1](#ch17.xhtml#automate_programs_using_com_scripting_in).

## See Also

- [Recipe 17.1, "Automate Programs Using COM Scripting Interfaces"](#ch17.xhtml#automate_programs_using_com_scripting_in)

# 27.8 Open or Close Ports in the Windows Firewall

## Problem

You want to open or close ports in the Windows Firewall.

## Solution

To open or close ports in the Windows Firewall, use the `LocalPolicy.CurrentProfile.GloballyOpenPorts` collection of the `HNetCfg.FwMgr` COM object.

To open a port, create a `HNetCfg.FWOpenPort` COM object to represent the port, and then add it to the `GloballyOpenPorts` collection:

```powershell
$PROTOCOL_TCP = 6
$firewall = New-Object -com HNetCfg.FwMgr
$port = New-Object -com HNetCfg.FWOpenPort

$port.Name = "Webserver at 8080"
$port.Port = 8080
$port.Protocol = $PROTOCOL_TCP

$firewall.LocalPolicy.CurrentProfile.GloballyOpenPorts.Add($port)
```

To close a port, remove it from the `GloballyOpenPorts` collection:

```powershell
$PROTOCOL_TCP = 6
$firewall.LocalPolicy.CurrentProfile.GloballyOpenPorts.Remove(8080, $PROTOCOL_TCP)
```

## Discussion

The `HNetCfg.FwMgr` COM object provides programmatic access to the Windows Firewall in Windows XP SP2 and later. The `LocalPolicy.CurrentProfile` property provides the majority of its functionality.

For more information about managing the Windows Firewall through its COM API, visit [the Microsoft documentation site](http://msdn.microsoft.com) and search for "Using Windows Firewall API." The documentation provides examples in VBScript but gives a useful overview of the functionality available.

For more information about working with COM objects in PowerShell, see [Recipe 17.1](#ch17.xhtml#automate_programs_using_com_scripting_in).

## See Also

- [Recipe 17.1, "Automate Programs Using COM Scripting Interfaces"](#ch17.xhtml#automate_programs_using_com_scripting_in)

# 27.9 Program: List All Installed Software

The best place to find information about currently installed software is actually from the place that stores information about how to uninstall it: the `HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall` registry key.

Each child of that registry key represents a piece of software you can uninstall—traditionally through the Add/Remove Programs entry in the Control Panel. In addition to the `DisplayName` of the application, other useful properties usually exist (depending on the application). Examples include `Publisher`, `UninstallString`, and `HelpLink`.

To see all the properties available from software installed on your system, type the following:

```powershell
$properties = Get-InstalledSoftware |
    ForEach-Object { $_.PsObject.Properties }

$properties | Select-Object Name | Sort-Object -Unique Name
```

This lists all properties mentioned by at least one installed application (although very few are shared by all installed applications).

To work with this data, though, you first need to retrieve it. [Example 27-3](#ch27.xhtml#get-installedsoftwareps1) provides a script to list all installed software on the current system, returning all information as properties of PowerShell objects.

## Example 27-3. Get-InstalledSoftware.ps1
