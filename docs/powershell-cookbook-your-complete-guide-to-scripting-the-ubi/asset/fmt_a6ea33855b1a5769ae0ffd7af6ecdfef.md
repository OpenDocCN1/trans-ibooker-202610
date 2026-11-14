
```
PS > $errorActionPreference = "Continue"
PS > trap { "Caught an error"; continue }; Get-WarningsAndErrors
WARNING: Warning: About to generate an error
Get-WarningsAndErrors: Error: You are running this script
Caught an error
```

Within a `catch` block or `trap` statement, the `$_` (or `$PSItem`) variable represents the current exception or error being processed.

Unlike the `try` statement, the `trap` statement handles terminating errors for anything in the scope that defines it. For more information about scopes, see [Recipe 3.6](#ch03.xhtml#control_access_and_scope_of_variables_an).

### Note

After handling an error, you can also remove it from the system's error collection by typing `$error.RemoveAt(0)`.

For more information about PowerShell's automatic variables, type `Get-Help about_automatic_variables`. For more information about error management in PowerShell, see [“Managing Errors”](#app01.xhtml#managing_errors). For more detailed information about the valid settings of these preference variables, see [Appendix A](#app01.xhtml#powershell_language_and_environment).

## See Also

[Recipe 1.2, “Run Programs, Scripts, and Existing Tools”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

[Recipe 3.6, “Control Access and Scope of Variables and Other Items”](#ch03.xhtml#control_access_and_scope_of_variables_an)

[“Managing Errors”](#app01.xhtml#managing_errors)

[Appendix A, *PowerShell Language and Environment*](#app01.xhtml#powershell_language_and_environment)

# 15.7 Output Warnings, Errors, and Terminating Errors

## Problem

You want your script to notify its caller of a warning, error, or terminating error.

## Solution

To write warnings and errors, use the `Write-Warning` and `Write-Error` cmdlets, respectively. Use the `throw` statement to generate a terminating error.

## Discussion

When you need to notify the caller of your script about an unusual condition, the `Write-Warning`, `Write-Error`, and `throw` statements are the way to do it. If your user should consider the message as more of a warning, use the `Write-Warning` cmdlet. If your script encounters an error (but can reasonably continue past that error), use the `Write-Error` cmdlet. If the error is fatal and your script simply cannot continue, use a `throw` statement.

For more information on generating these errors and handling them when thrown by other scripts, see [Recipe 15.6](#ch15.xhtml#handle_warnings_errors_and_terminating_e). For more information about error management in PowerShell, see [“Managing Errors”](#app01.xhtml#managing_errors). For more information about running scripts, see [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools).

## See Also

[Recipe 1.2, “Run Programs, Scripts, and Existing Tools”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

[Recipe 15.6, “Handle Warnings, Errors, and Terminating Errors”](#ch15.xhtml#handle_warnings_errors_and_terminating_e)

[“Managing Errors”](#app01.xhtml#managing_errors)

# 15.8 Analyze a Script's Performance Profile

## Problem

You have an interactive or time-intensive script and want to see which parts are slowing it down.

## Solution

Install the `PSProfiler` module from the PowerShell Gallery, and use the `Measure-Script` command to see the lines of your script that are consuming the most time. In this example, we are investigating `burn-console-original.ps1`, a fun little script that displays a demoscene-like fire effect in the PowerShell console:

```
PS > Install-Module -Name PSProfiler -Scope CurrentUser
PS > $profileData = Measure-Script (Get-Command burn-console-original.ps1).Path
PS > $profileData | Sort-Object ExecutionTime -Desc | Select -First 18

 Count  Line       Time Taken Statement
 -----  ----       ---------- ---------
     1   252    00:12.7965373 . main
     1    57    00:12.6452424 $totalTime = Measure-Command {
    26    60    00:11.1477132 updateBuffer
    26    61    00:01.4885363 updateScreen
111020   146    00:00.4423771 $nextScreen[$row, $column] = `
104312   122    00:00.3652381 $tempWorkingBuffer[$baseOffset - $windowWidth] = `
104312   110    00:00.3542900 $colour += $screenBuffer[$baseOffset + $windowWidth]
104312   103    00:00.3332991 $baseOffset = ($windowWidth * $row) + $column
104312   108    00:00.3325509 $colour += $screenBuffer[$baseOffset - 1]
104312   107    00:00.3277796 $colour = $screenBuffer[$baseOffset]
104312   109    00:00.3223024 $colour += $screenBuffer[$baseOffset + 1]
104312   111    00:00.2604536 $colour /= 4.0
102983   116    00:00.2573369 if($colour -le 70) { $colour -= 3 }
101458   117    00:00.2492673 if($colour -lt 20) { $colour -= 1 }
100910   118    00:00.2466116 if($colour -lt 0) { $colour = 0 }
    26   152    00:00.0921347 $host.UI.RawUI.SetBufferContents($origin, $nextScreen)
     1    46    00:00.0609062 clear-host
     1    49    00:00.0515547 generatePalette
```

## Discussion

When you write scripts that cause the user or other systems to wait on the result, you may sometimes feel that your script could benefit from better performance.

The first rule for tackling performance problems is to measure the problem. Unless you can guide your optimization efforts with hard performance data, you're almost certainly directing your efforts to the wrong spots. Random cute performance improvements will quickly turn your code into an unreadable mess, often with no appreciable performance gain! Low-level optimization has its place, but it should always be guided by hard data that supports it.

The way to obtain hard performance data is from a profiler. PowerShell doesn't ship with a script profiler, but it does ship with the features that let the community write one! The `PSProfiler` module was written in collaboration with several members of the PowerShell team, and is an excellent addition to your toolchest.

## See Also

[Recipe 1.2, “Run Programs, Scripts, and Existing Tools”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

[Recipe 1.29, “Find and Install Additional PowerShell Scripts and Modules”](#ch01.xhtml#install_module_gallery)

# Chapter 16. Environmental Awareness

## 16.0 Introduction

While many of your scripts will be designed to work in isolation, you'll often find it helpful to give your script information about its execution environment: its name, current working directory, environment variables, common system paths, and more.

PowerShell offers several ways to get at this information—from its cmdlets and built-in variables to features that it offers from the .NET Framework.

## 16.1 View and Modify Environment Variables

### Problem

You want to interact with your system's environment variables.

### Solution

To interact with environment variables, access them in almost the same way that you access regular PowerShell variables. The only difference is that you place `env:` between the dollar sign ($) and the variable name:

```
PS > $env:Username
Lee
```

You can modify environment variables this way, too. For example, to temporarily add the current directory to the path:

```
PS > Invoke-DemonstrationScript
Invoke-DemonstrationScript.ps1: The term 'Invoke-DemonstrationScript.ps1' is not
recognized as a name of a cmdlet, function, script file, or executable program.
Check the spelling of the name, or if a path was included, verify that the path
is correct and try again.

Suggestion [3,General]: The command Invoke-DemonstrationScript.ps1 was not found,
but does exist in the current location. PowerShell does not load commands from
the current location by default. If you trust this command, instead type:
".\Invoke-DemonstrationScript.ps1". See "get-help about_Command_Precedence"
for more details.

PS > $env:PATH = $env:PATH + ".;"
PS > Invoke-DemonstrationScript
The script ran!
```

### Discussion
