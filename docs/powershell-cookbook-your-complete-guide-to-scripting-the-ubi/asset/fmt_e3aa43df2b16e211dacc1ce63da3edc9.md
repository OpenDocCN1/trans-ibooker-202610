
## Solution

To interact with advanced features of WMI objects, access their methods and properties.

### Advanced instance features

To get CIM instances related to a given instance (its *associators*), use the `Get-CimAssociatedInstance` cmdlet:

```
Get-CimInstance Win32_Service -Filter "Name='winmgmt'" |
    Get-CimAssociatedInstance
```

When dealing with WMI, use the `GetRelated()` method:

```
$instance = [Wmi] 'Win32_Service.Name="winmgmt"'
$instance.GetRelated()
```

When dealing with WMI (as opposed to CIM cmdlets), connectivity options play a major role in some scenarios. To change advanced scope options, access the `Scope.Options` property:

```
$system = [WmiClass] 'Win32_OperatingSystem'
$system.Scope.Options.EnablePrivileges = $true
$system.SetDateTime($system.ConvertFromDateTime("01/01/2007"))
```

### Advanced class features

To retrieve the properties and qualifiers of a class, access the `CimClassProperties` property:

```
$class = Get-CimClass Win32_Process
$class.CimClassProperties
```

### Advanced query feature

When dealing with WMI, advanced query options play a major role in some scenarios. To configure connection options on a query, such as Packet Privacy and Authentication, set the options on the `Scope` property:

```
$credential = Get-Credential
$query = [WmiSearcher] "SELECT * FROM IISWebServerSetting"
$query.Scope.Path = "\\REMOTE_COMPUTER\Root\MicrosoftIISV2"
$query.Scope.Options.Username = $credential.Username
$query.Scope.Options.Password = $credential.GetNetworkCredential().Password
$query.Scope.Options.Authentication = "PacketPrivacy"
$query.get() | Select-Object AnonymousUserName
```

## Discussion

The `[Wmi]`, `[WmiClass]`, and `[WmiSearcher]` type shortcuts return instances of .NET `System.Management.ManagementObject`, `System.Management.ManagementClass`, and `System.Management.ManagementObjectSearcher` classes, respectively.

As might be expected, the .NET Framework provides comprehensive support for WMI queries, with PowerShell providing an easier-to-use interface to that support. If you need to step outside the support offered directly by PowerShell, these classes in the .NET Framework provide an advanced outlet.

For more information about working with classes from the .NET Framework, see [Recipe 3.8](#ch03.xhtml#work_with_net_objects).

## See Also

[Recipe 3.8, “Work with .NET Objects”](#ch03.xhtml#work_with_net_objects)

# 28.7 Convert a VBScript WMI Script to PowerShell

## Problem

You want to perform a WMI or CIM task in PowerShell, but you can find only VBScript examples that demonstrate the solution to the problem.

## Solution

To accomplish the task of a script that retrieves data from a computer, use the `Get-CimInstance` cmdlet:

```
foreach($printer in Get-CimInstance -Computer COMPUTER Win32_Printer)
{
    ## Work with the properties
    $printer.Name
}
```

To accomplish the task of a script that calls methods on an instance, use the `[Wmi]` or `[WmiSearcher]` accelerators to retrieve the instances, and then call methods on the instances like you would call any other PowerShell method.

```
$service = [WMI] 'Win32_Service.Name="winmgmt"'
$service.ChangeStartMode("Manual")
$service.ChangeStartMode("Automatic")
```

Additionally, you can use the `Get-CimInstance` and `Invoke-CimMethod` cmdlets to do the same:

```
$service = Get-CimInstance Win32_Service -Filter "Name = 'winmgmt'"
$service | Invoke-CimMethod -Name ChangeStartMode -Arguments @{
    StartMode = "Manual" }
$service | Invoke-CimMethod -Name ChangeStartMode -Arguments @{
    StartMode = "Automatic" }
```

To accomplish the task of a script that calls methods on a class, use the `Invoke-CimMethod` cmdlet, or use the `[WmiClass]` accelerator to retrieve the class, and then call methods on the class like you would call any other PowerShell method:

```
Invoke-CimMethod -Class Win32_Process -Name Create -Arguments @{
    CommandLine = "notepad" }

$class = [WmiClass] "Win32_Process"
$class.Create("Notepad")
```

## Discussion

For many years, VBScript has been the preferred language that administrators use to access WMI data. Because of that, the vast majority of scripts available in books and on the internet come written in VBScript.

These scripts usually take one of three forms: retrieving data and accessing properties, calling methods of an instance, and calling methods of a class.

### Note

Although most WMI scripts on the internet accomplish unique tasks, PowerShell supports many of the traditional WMI tasks natively. If you want to translate a WMI example to PowerShell, first check that there aren’t any PowerShell cmdlets that might accomplish the task directly.

### Retrieving data

One of the most common uses of WMI is for data collection and system inventory tasks. A typical VBScript that retrieves data looks like [Example 28-3](#ch28.xhtml#retrieving_printer_information_from_wmi_).

#### Example 28-3. Retrieving printer information from WMI using VBScript

```
strComputer = "."
Set objWMIService = GetObject("winmgmts:" _
    & "{impersonationLevel=impersonate}!\\" & strComputer & "\root\cimv2")

Set colInstalledPrinters = objWMIService.ExecQuery _
    ("Select * from Win32_Printer")

For Each objPrinter in colInstalledPrinters
    Wscript.Echo "Name: " & objPrinter.Name
    Wscript.Echo "Location: " & objPrinter.Location
    Wscript.Echo "Default: " & objPrinter.Default
Next
```

The first three lines prepare a WMI connection to a given computer and namespace. The next two lines of code prepare a WMI query that requests all instances of a class. The `For Each` block loops over all the instances, and the `objPrinter.Property` statements interact with properties on those instances.

In PowerShell, the `Get-CimInstance` cmdlet takes care of most of that by retrieving all instances of a class from the computer and namespace that you specify. The first five lines of code then become:

```
$installedPrinters = Get-CimInstance Win32_Printer -ComputerName computer
```

If you need to specify a different computer, namespace, or query restriction, the `Get-CimInstance` cmdlets supports those through optional parameters.

In PowerShell, the `For Each` block becomes:

```
foreach($printer in $installedPrinters)
{
    $printer.Name
    $printer.Location
    $printer.Default
}
```

Notice that we spend the bulk of the PowerShell conversion of this script showing how to access properties. If you don’t actually need to work with the properties (and only want to display them for reporting purposes), PowerShell’s formatting commands simplify that even further:

```
Get-CimInstance Win32_Printer -ComputerName computer |
    Format-List Name,Location,Default
```

For more information about working with the `Get-CimInstance` cmdlet, see [Recipe 28.1](#ch28.xhtml#access_windows_management_instrumentatio).

### Calling methods on an instance

Although data retrieval scripts form the bulk of WMI management examples, another common task is to call methods of an instance that invoke actions.

For example, [Example 28-4](#ch28.xhtml#changing_the_startup_type_of_a_service_f) changes the startup type of a service.

#### Example 28-4. Changing the startup type of a service from WMI using VBScript

```
strComputer = "."
Set objWMIService = GetObject("winmgmts:" _
    & "{impersonationLevel=impersonate}!\\" & strComputer & "\root\cimv2")

Set colServiceList = objWMIService.ExecQuery _
    ("Select * from Win32_Service where StartMode = 'Manual'")

For Each objService in colServiceList
    errReturnCode = objService.ChangeStartMode("Disabled")
Next
```

The first three lines prepare a WMI connection to a given computer and namespace. The next two lines of code prepare a WMI query that requests all instances of a class and adds an additional filter (`StartMode = 'Manual'`) to the query. The `For Each` block loops over all the instances, and the `objService.ChangeStartMode(...)` statement calls the `ChangeStartMode()` method on the service.
