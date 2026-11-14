
Although you can assign new property values to the objects output by `Get-CimInstance`, changes you make ultimately are not reflected in the permanent system state, as this example shows:

```
PS > $bootVolume = Get-CimInstance Win32_LogicalDisk |
    Where-Object { $_.DeviceID -eq 'C:' }

PS > $bootVolume

DeviceID     : C:
DriveType    : 3
ProviderName :
FreeSpace    : 10587656192
Size         : 34357637120
VolumeName   : OS

PS > $bootVolume.VolumeName = "Boot Volume"

PS > Get-CimInstance Win32_LogicalDisk |
    Where-Object { $_.DeviceID -eq 'C:' }


DeviceID     : C:
DriveType    : 3
ProviderName :
FreeSpace    : 10587652096
Size         : 34357637120
VolumeName   : OS
```

Instead, the `Set-CimInstance` cmdlet lets you permanently modify values of WMI and CIM instances. While the `Set-CimInstance` cmdlet supports WMI instances as pipeline input, you can also use the `-Query` parameter to select the instance you want to modify:

```
Set-CimInstance -Query "SELECT * FROM Win32_LogicalDisk WHERE DeviceID='C:'" `
    -Property @{ VolumeName="OS" }
```

To determine which properties can be modified on an instance, you need to investigate the CIM class that defines it. Each CIM class has a `CimClassProperties` collection, and each property has a `Qualifiers` collection. If `Write` is one of the qualifiers, then that property is writeable:

```
PS > Get-CimClass Win32_LogicalDisk | ForEach-Object CimClassProperties

(...)
Name               : VolumeName
Value              :
CimType            : String
Flags              : Property, NullValue
Qualifiers         : {MappingStrings, read, write}
ReferenceClassName :

Name               : VolumeSerialNumber
Value              :
CimType            : String
Flags              : Property, ReadOnly, NullValue
Qualifiers         : {MappingStrings, read}
ReferenceClassName :
(...)
```

To automatically see all writeable classes in the `ROOT\CIMV2` namespace, simply run this snippet of PowerShell script:

```
Get-CimClass Win32_LogicalDisk | ForEach-Object {
    $_.CimClassName; $_.CimClassProperties |
    Where-Object { $_.Qualifiers["Write"] } | ForEach-Object Name
}
```

## See Also

[Recipe 28.1, “Access Windows Management Instrumentation and CIM Data”](#ch28.xhtml#access_windows_management_instrumentatio)

[Appendix G, *WMI Reference*](#app07.xhtml#wmi_reference)

# 28.3 Invoke a Method on a WMI Instance or Class

## Problem

You want to invoke a method supported by a WMI instance or class.

## Solution

Use the `Invoke-CimMethod` cmdlet:

```
PS > Invoke-CimMethod -Class Win32_Process -Name Create -Arguments @{
    CommandLine = "notepad" }
(notepad starts)

ProcessId                                ReturnValue PSComputerName
---------                                ----------- --------------
     5976                                          0 leeholmes1c23
```

## Discussion

As with .NET types, WMI classes describe the functionality and features of a related set of items. For example, the `Win32_Process` class describes the features and behavior of an entity called an *operating system process*. When WMI or CIM returns information about a specific *operating system process*, that is called an *instance*.

As with static methods on .NET types, many WMI and CIM classes offer methods that relate broadly to the entity they try to represent. For example, the `Win32_Process` class defines methods to start processes, stop them, and more. To invoke any of these methods, call the `Invoke-CimMethod` cmdlet.

While you may already know the method you want to call, PowerShell also offers a way to see the methods exposed by WMI and CIM classes on your system. Each WMI class has a `CimClassMethods` collection, and reviewing that collection lists all methods supported by that class. The following snippet lists all methods supported by the `Win32_Process` class:

```
Get-CimClass Win32_Process | ForEach-Object CimClassMethods
```

Once you find a method you want to invoke, you can access the `Parameters` property to see how to invoke the method:

```
PS > $methods = Get-CimClass Win32_Process | ForEach-Object CimClassMethods
PS > $methods | Where-Object Name -eq Create | ForEach-Object Parameters

Name                       CimType Qualifiers
----                       ------- ----------
CommandLine                 String {ID, In, MappingStrings}
CurrentDirectory            String {ID, In, MappingStrings}
ProcessStartupInformation Instance {EmbeddedInstance, ID, In, MappingStrings}
ProcessId                   UInt32 {ID, MappingStrings, Out}
```

In addition to the `Invoke-CimMethod` cmdlet, the `[WmiClass]` type shortcut also lets you refer to a WMI class and invoke its methods:

```
$processClass = [WmiClass] "Win32_Process"
$processClass.Create("notepad.exe")
```

This method, however, does not easily support customization of impersonation, authentication, or privilege restrictions.

For more information about working with WMI classes, see [Recipe 28.1](#ch28.xhtml#access_windows_management_instrumentatio).

## See Also

[Recipe 28.1, “Access Windows Management Instrumentation and CIM Data”](#ch28.xhtml#access_windows_management_instrumentatio)

[Appendix G, *WMI Reference*](#app07.xhtml#wmi_reference)

# 28.4 Program: Determine Properties Available to WMI and CIM Filters

When you want to access a specific WMI or CIM instance with PowerShell, you might at first struggle to determine what properties WMI or CIM lets you search on. These properties are called *key properties* on the class. [Example 28-1](#ch28.xhtml#get-wmiclasskeypropertyps1) gets all the properties you can use in a WMI filter for a given class.

### Example 28-1. Get-WmiClassKeyProperty.ps1

```
##############################################################################
##
## Get-WmiClassKeyProperty
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Get all of the properties that you may use in a WMI filter for a given class.

.EXAMPLE

PS > Get-WmiClassKeyProperty Win32_Process
Handle

#>

param(
    ## The WMI class to examine
    [WmiClass] $WmiClass
)

Set-StrictMode -Version 3

## WMI classes have properties
foreach($currentProperty in $wmiClass.Properties)
{
    ## WMI properties have qualifiers to explain more about them
    foreach($qualifier in $currentProperty.Qualifiers)
    {
        ## If it has a 'Key' qualifier, then you may use it in a filter
        if($qualifier.Name -eq "Key")
        {
            $currentProperty.Name
        }
    }
}
```

For more information about running scripts, see [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools).

## See Also

[Recipe 1.2, “Run Programs, Scripts, and Existing Tools”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

# 28.5 Search for the WMI or CIM Class to Accomplish a Task

## Problem

You want to find the WMI or CIM class that can help you accomplish a task.

## Solution

Use the `Get-CimClass` cmdlet, using wildcards to filter for keywords of interest:

```
PS > Get-CimClass *restore* -namespace root/default

   NameSpace: ROOT/default

CimClassName           CimClassMethods      CimClassProperties
------------           ---------------      ------------------
SystemRestoreConfig    {}                   {DiskPercent, MyKey, RPGlobalInte...
SystemRestore          CreateRestorePoint…  {CreationTime, Description, Event...
```

## Discussion

Along with WMI’s huge scope comes a related problem: finding the WMI or CIM class that accomplishes your task. To help you learn what WMI classes are available, [Appendix G](#app07.xhtml#wmi_reference) provides a helpful listing of the most common ones.
