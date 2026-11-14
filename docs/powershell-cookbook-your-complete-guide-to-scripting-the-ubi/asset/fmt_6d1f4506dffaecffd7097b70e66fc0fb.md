
## Adding the `Add-Type` Call

Now we add the call to `Add-Type`. This signature becomes the building block for a new class, so we only need to give it a name. To prevent its name from colliding with other classes with the same name, we also put it in a namespace. The name of our script is a good choice:

```
## Get-PrivateProfileString.ps1
param(
    $Path,
    $Category,
    $Key)

$signature = @'
[DllImport("kernel32.dll")]
public static extern uint GetPrivateProfileString(
    string lpAppName,
    string lpKeyName,
    string lpDefault,
    StringBuilder lpReturnedString,
    uint nSize,
    string lpFileName);
'@

$type = Add-Type -MemberDefinition $signature `
    -Name Win32Utils -Namespace GetPrivateProfileString `
    -PassThru

$null
```

When we try to run this script, though, we get an error:

```
The type or namespace name 'StringBuilder' could not be found (are you missing a
using directive or an assembly reference?)
c:\Temp\obozeqo1.0.cs(12) :    string lpDefault,
c:\Temp\obozeqo1.0.cs(13) : >>>    StringBuilder lpReturnedString,
c:\Temp\obozeqo1.0.cs(14) :    uint nSize,
```

Indeed we are missing something. The `StringBuilder` class is defined in the `System.Text` namespace, which requires a `using` directive to be placed at the top of the program by the class definition. Since we're letting PowerShell define the type for us, we can either rename `StringBuilder` to `System.Text.StringBuilder` or add a `-UsingNamespace` parameter to have PowerShell add the `using` statement for us.

> **Note**
>
> PowerShell adds references to the `System` and `System.Runtime.InteropServices` namespaces by default.

Let's do the latter:

```
## Get-PrivateProfileString.ps1
param(
    $Path,
    $Category,
    $Key)

$signature = @'
[DllImport("kernel32.dll")]
public static extern uint GetPrivateProfileString(
    string lpAppName,
    string lpKeyName,
    string lpDefault,
    StringBuilder lpReturnedString,
    uint nSize,
    string lpFileName);
'@

$type = Add-Type -MemberDefinition $signature `
    -Name Win32Utils -Namespace GetPrivateProfileString `
    -Using System.Text -PassThru

$builder = New-Object System.Text.StringBuilder 1024
$null = $type::GetPrivateProfileString($category,
    $key, "", $builder, $builder.Capacity, $path)

$builder.ToString()
```

Now we can plug in all of the necessary parameters. The `GetPrivateProfileString` function puts its output in a `StringBuilder`, so we'll have to feed it one and return its contents. This gives us the script shown in [Example 17-3](#ch17.xhtml#get-privateprofilestringps1).

```
PS > Get-PrivateProfileString c:\windows\system32\tcpmon.ini `
   "<Generic Network Card>" Name
Generic Network Card
```

So now we have it. With just a few lines of code, we've defined and invoked a Win32 API call.

For more information about working with classes from the .NET Framework, see [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools).

## See Also

- [Recipe 1.2, "Run Programs, Scripts, and Existing Tools"](#ch01.xhtml#run_programs_scripts_and_existing_tools)
- [Recipe 17.5, "Program: Invoke Simple Windows API Calls"](#ch17.xhtml#program_invoke_native_windows_api_calls)
- [Recipe 17.6, "Define or Extend a .NET Class"](#ch17.xhtml#define_extend_net_class)
- [Recipe 17.8, "Access a .NET SDK Library"](#ch17.xhtml#access_a_net_sdk_library)

# 17.5 Program: Invoke Simple Windows API Calls

There are times when neither PowerShell's cmdlets nor its scripting language directly support a feature you need. In most of those situations, PowerShell's direct support for the .NET Framework provides another avenue to let you accomplish your task. In some cases, though, even the .NET Framework doesn't support a feature you need to resolve a problem, and the only solution is to access the core Windows APIs.

For complex API calls (ones that take highly structured data), the solution is to use the `Add-Type` cmdlet (or write a PowerShell cmdlet) that builds on the Platform Invoke (P/Invoke) support in the .NET Framework. The P/Invoke support in the .NET Framework is designed to let you access core Windows APIs directly.

Although it's possible to determine these P/Invoke definitions yourself, it's usually easiest to build on the work of others. If you want to know how to call a specific Windows API from a .NET language, the [P/Invoke .NET website](http://www.pinvoke.net) is the best place to start.

If the API you need to access is straightforward (one that takes and returns only simple data types), however, [Example 17-4](#ch17.xhtml#invoke-windowsapips1) can do most of the work for you.

### Example 17-4. Invoke-WindowsApi.ps1
