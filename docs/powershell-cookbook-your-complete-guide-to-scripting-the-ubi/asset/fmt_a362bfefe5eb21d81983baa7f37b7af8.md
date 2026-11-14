
# Get-RemoteRegistryKeyProperty.ps1

```powershell
##############################################################################
##
## Get-RemoteRegistryKeyProperty
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Get the value of a remote registry key property

.EXAMPLE

PS > $registryPath =
     "HKLM:\software\Microsoft\PowerShell\1\ShellIds\Microsoft.PowerShell"
PS > Get-RemoteRegistryKeyProperty LEE-DESK $registryPath ExecutionPolicy

#>

param(
    ## The computer that you wish to connect to
    [Parameter(Mandatory = $true)]
    $ComputerName,

    ## The path to the registry item to retrieve
    [Parameter(Mandatory = $true)]
    $Path,

    ## The specific property to retrieve
    $Property = "*"
)

Set-StrictMode -Version 3

## Validate and extract out the registry key
if($path -match "^HKLM:\\(.*)")
{
    $baseKey = [Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey(
        "LocalMachine", $computername)
}
elseif($path -match "^HKCU:\\(.*)")
{
    $baseKey = [Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey(
        "CurrentUser", $computername)
}
else
{
    Write-Error ("Please specify a fully-qualified registry path " +
        "(i.e.: HKLM:\Software) of the registry key to open.")
    return
}

## Open the key
$key = $baseKey.OpenSubKey($matches[1])
$returnObject = New-Object PsObject

## Go through each of the properties in the key
foreach($keyProperty in $key.GetValueNames())
{
    ## If the property matches the search term, add it as a
    ## property to the output
    if($keyProperty -like $property)
    {
        $returnObject |
            Add-Member NoteProperty $keyProperty $key.GetValue($keyProperty)
    }
}

## Return the resulting object
$returnObject

## Close the key and base keys
$key.Close()
$baseKey.Close()
```

For more information about running scripts, see [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools).

## See Also

[Recipe 1.2, “Run Programs, Scripts, and Existing Tools”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

# 21.15 Program: Set Properties of Remote Registry Keys

Although PowerShell doesn’t directly let you access and manipulate the registry of a remote computer, it still supports this by working with the .NET Framework. The functionality exposed by the .NET Framework is a bit more developer-oriented than we want, so we can instead use a script to make it easier to work with.

[Example 21-8](#ch21.xhtml#set-remoteregistrykeypropertyps1) lets you set the value of a property on a given remote registry key. For this script to succeed, the target computer must have the remote registry service enabled and running.

## Example 21-8. Set-RemoteRegistryKeyProperty.ps1

```powershell
##############################################################################
##
## Set-RemoteRegistryKeyProperty
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Set the value of a remote registry key property

.EXAMPLE

PS >$registryPath =
    "HKLM:\software\Microsoft\PowerShell\1\ShellIds\Microsoft.PowerShell"
PS >Set-RemoteRegistryKeyProperty LEE-DESK $registryPath `
      "ExecutionPolicy" "RemoteSigned"

#>

param(
    ## The computer to connect to
    [Parameter(Mandatory = $true)]
    $ComputerName,

    ## The registry path to modify
    [Parameter(Mandatory = $true)]
    $Path,

    ## The property to modify
    [Parameter(Mandatory = $true)]
    $PropertyName,

    ## The value to set on the property
    [Parameter(Mandatory = $true)]
    $PropertyValue
)

Set-StrictMode -Version 3

## Validate and extract out the registry key
if($path -match "^HKLM:\\(.*)")
{
    $baseKey = [Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey(
        "LocalMachine", $computername)
}
elseif($path -match "^HKCU:\\(.*)")
{
    $baseKey = [Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey(
        "CurrentUser", $computername)
}
else
{
    Write-Error ("Please specify a fully-qualified registry path " +
        "(i.e.: HKLM:\Software) of the registry key to open.")
    return
}

## Open the key and set its value
$key = $baseKey.OpenSubKey($matches[1], $true)
$key.SetValue($propertyName, $propertyValue)

## Close the key and base keys
$key.Close()
$baseKey.Close()
```

For more information about running scripts, see [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools).

## See Also

[Recipe 1.2, “Run Programs, Scripts, and Existing Tools”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

# 21.16 Discover Registry Settings for Programs

## Problem

You want to automate the configuration of a program, but that program doesn’t document its registry configuration settings.

## Solution

To discover a registry setting for a program, use the Sysinternals Process Monitor to observe registry access by that program. Process Monitor is available at the [Sysinternals download site](https://oreil.ly/KGiqe).

## Discussion

In an ideal world, all programs would fully support command-line administration and configuration through PowerShell cmdlets. Many programs do not, however, so the solution is to look through their documentation in the hope that they list the registry keys and properties that control their settings. While many programs document their registry configuration settings, many still do not.

Although these programs may not document their registry settings, you can usually observe their registry access activity to determine the registry paths they use. To illustrate this, we’ll use the Sysinternals Process Monitor to discover Windows PowerShell’s execution policy configuration keys. Although Windows PowerShell documents these keys *and* makes its automated configuration a breeze, this example illustrates the general technique.

#### Note

PowerShell (as opposed to Windows PowerShell) uses a configuration file (`powershell.config.json`) for these settings, rather than the registry. If you want to follow along, be sure to use Windows PowerShell!

### Launch and configure Process Monitor

Once you’ve downloaded Process Monitor, the first step is to filter its output to include only the program you’re interested in. By default, Process Monitor logs almost all registry and file activity on the system.

First, launch Process Monitor, and then press Ctrl+E (or click the magnifying glass icon) to temporarily prevent it from capturing any data (see [Figure 21-2](#ch21.xhtml#process_monitor_ready_to_capture)). Next, press Ctrl+X (or click the white sheet with an eraser icon) to clear the extra information that it captured automatically. Finally, drag the target icon and drop it on top of the application in question. You can press Ctrl+L (or click the funnel icon) to see the filter that Process Monitor now applies to its output.

![](Images/wps4_2102.png)

### Prepare to manually set the configuration option

Next, prepare to manually set the program’s configuration option. Usually, this means typing and clicking all the property settings, but just not clicking OK or Apply. For this PowerShell example, type the `Set-ExecutionPolicy` command line, but do not press Enter (see [Figure 21-3](#ch21.xhtml#preparing_to_apply_the_configuration_opt)).

![](Images/wps4_2103.png)

### Tell Process Monitor to begin capturing information

Switch to the Process Monitor window, and then press Ctrl+E (or click the magnifying glass icon). Process Monitor now captures all registry access for the program in question.

### Manually set the configuration option

Click OK, Apply, or whatever action it takes to actually complete the program’s configuration. For the PowerShell example, this means pressing Enter.

### Tell Process Monitor to stop capturing information
