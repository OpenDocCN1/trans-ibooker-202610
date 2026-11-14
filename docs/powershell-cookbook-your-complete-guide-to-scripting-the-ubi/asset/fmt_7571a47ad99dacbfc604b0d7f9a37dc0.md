
# Working with Remote Registry

To work with the registry of a remote computer, use the scripts provided in this chapter: `Get-RemoteRegistryChildItem` (Recipe 21.13), `Get-RemoteRegistryKeyProperty` (Recipe 21.14), and `Set-RemoteRegistryKeyProperty` (Recipe 21.15). These scripts require that the remote computer has the remote registry service enabled and running. Example 21-5 updates the PowerShell execution policy of a remote machine.

## Example 21-5: Setting the PowerShell Execution Policy of a Remote Machine

```
PS > $registryPath = "HKLM:\Software\Microsoft\PowerShell\1"
PS > Get-RemoteRegistryChildItem LEE-DESK $registryPath

SKC  VC Name                           Property
---  -- ----                           --------
  0   1 1033                           {Install}
  0   5 PowerShellEngine               {ApplicationBase, ConsoleHost...
  2   0 PowerShellSnapIns              {}
  1   0 ShellIds                       {}

PS > Get-RemoteRegistryChildItem LEE-DESK $registryPath\ShellIds

SKC  VC Name                           Property
---  -- ----                           --------
  0   2 Microsoft.PowerShell           {Path, ExecutionPolicy}

PS > $registryPath = "HKLM:\Software\Microsoft\PowerShell\1\" +
    "ShellIds\Microsoft.PowerShell"

PS > Get-RemoteRegistryKeyProperty LEE-DESK $registryPath ExecutionPolicy

ExecutionPolicy
---------------
Unrestricted

PS > Set-RemoteRegistryKeyProperty LEE-DESK $registryPath `
    "ExecutionPolicy" "RemoteSigned"

PS > Get-RemoteRegistryKeyProperty LEE-DESK $registryPath ExecutionPolicy

ExecutionPolicy
---------------
RemoteSigned
```

## Discussion

Although this specific task is perhaps better solved through PowerShell's Group Policy support, it demonstrates a useful scenario that includes both remote registry exploration and modification.

If the remote computer does not have the remote registry service running (but does have WMI enabled), you can use WMI's `StdRegProv` class to work with the registry as well. The following example demonstrates how to get and set the registry key that controls Remote Desktop:

```
$HKEY_CLASSES_ROOT = [Convert]::ToUInt32(80000000, 16)
$HKEY_CURRENT_USER = [Convert]::ToUInt32(80000001, 16)
$HKEY_LOCAL_MACHINE = [Convert]::ToUInt32(80000002, 16)
$HKEY_USERS = [Convert]::ToUInt32(80000003, 16)
$HKEY_CURRENT_CONFIG = [Convert]::ToUInt32(80000005, 16)

## Connect to the registry via WMI
$reg = Get-CimClass -ComputerName LEE-DESK `
    -Namespace root\default StdRegProv

## Get and set DWORD values on the remote machine
$reg | Invoke-CimMethod -Name GetDWORDValue -Arguments @{
    hDefKey = $HKEY_LOCAL_MACHINE;
    sSubKeyName = "SYSTEM\CurrentControlSet\Control\Terminal Server";
    sValueName = "fDenyTSConnections"
}

$reg | Invoke-CimMethod -Name SetDWORDValue -Arguments @{
    hDefKey = $HKEY_LOCAL_MACHINE;
    sSubKeyName = "SYSTEM\CurrentControlSet\Control\Terminal Server";
    sValueName = "fDenyTSConnections";
    uValue = 0
}
```

For more information about the `Get-RemoteRegistryChildItem`, `Get-RemoteRegistryKeyProperty`, and `Set-RemoteRegistryKeyProperty` scripts, see Recipes 21.13, 21.14, and 21.15.

## See Also

- Recipe 21.13: "Program: Get Registry Items from Remote Machines"
- Recipe 21.14: "Program: Get Properties of Remote Registry Keys"
- Recipe 21.15: "Program: Set Properties of Remote Registry Keys"

# 21.13 Program: Get Registry Items from Remote Machines

Although PowerShell doesn't directly let you access and manipulate the registry of a remote computer, it still supports this by working with the .NET Framework. The functionality exposed by the .NET Framework is a bit more developer-oriented than we want, so we can instead use a script to make it easier to work with.

Example 21-6 lets you list child items in a remote registry key, much like you do on the local computer. For this script to succeed, the target computer must have the remote registry service enabled and running.

## Example 21-6: Get-RemoteRegistryChildItem.ps1

```
##############################################################################
##
## Get-RemoteRegistryChildItem
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Get the list of subkeys below a given key on a remote computer.

.EXAMPLE

Get-RemoteRegistryChildItem LEE-DESK HKLM:\Software

#>

param(
    ## The computer that you wish to connect to
    [Parameter(Mandatory = $true)]
    $ComputerName,

    ## The path to the registry items to retrieve
    [Parameter(Mandatory = $true)]
    $Path
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

## Retrieve all of its children
foreach($subkeyName in $key.GetSubKeyNames())
{
    ## Open the subkey
    $subkey = $key.OpenSubKey($subkeyName)

    ## Add information so that PowerShell displays this key like regular
    ## registry key
    $returnObject = [PsObject] $subKey
    $returnObject | Add-Member NoteProperty PsChildName $subkeyName
    $returnObject | Add-Member NoteProperty Property $subkey.GetValueNames()

    ## Output the key
    $returnObject

    ## Close the child key
    $subkey.Close()
}

## Close the key and base keys
$key.Close()
$baseKey.Close()
```

For more information about running scripts, see Recipe 1.2.

## See Also

- Recipe 1.2: "Run Programs, Scripts, and Existing Tools"

# 21.14 Program: Get Properties of Remote Registry Keys

Although PowerShell doesn't directly let you access and manipulate the registry of a remote computer, it still supports this by working with the .NET Framework. The functionality exposed by the .NET Framework is a bit more developer-oriented than we want, so we can instead use a script to make it easier to work with.

Example 21-7 lets you get the properties (or a specific property) from a given remote registry key. For this script to succeed, the target computer must have the remote registry service enabled and running.

## Example 21-7: Get-RemoteRegistryKeyProperty.ps1
