
# 11.15 Support `-WhatIf` and `-Confirm` Parameters

## Problem

You want to support the standard `-WhatIf` and `-Confirm` parameters and access cmdlet-centric support in the PowerShell engine.

## Solution

Ensure that your script or function declares the `[CmdletBinding()]` attribute, and then access engine features through the `$psCmdlet` automatic variable.

```
function Invoke-MyAdvancedFunction
{
    [CmdletBinding(SupportsShouldProcess = $true)]
    param()

    if($psCmdlet.ShouldProcess("test.txt", "Remove Item"))
    {
        "Removing test.txt"
    }

    Write-Verbose "Verbose Message"
}
```

## Discussion

When a script or function progresses to an *advanced function*, PowerShell defines an additional `$psCmdlet` automatic variable. This automatic variable exposes support for the `-WhatIf` and `-Confirm` automatic parameters. If your command defined parameter sets, it also exposes the parameter set name that PowerShell selected based on the user's choice of parameters. For more information about advanced functions, see [Recipe 11.11](#ch11.xhtml#access_arguments_of_a_script_function_or).

To support the `-WhatIf` and `-Confirm` parameters, add the `[CmdletBinding(SupportsShouldProcess = $true)]` attribute inside of your script or function. You should support this on any scripts or functions that modify system state, as they let your users investigate what your script will do before actually doing it. Then, you simply surround the portion of your script that changes the system with an `if($psCmdlet.ShouldProcess(...) ) { }` block. [Example 11-14](#ch11.xhtml#adding_support_whatif_confirm) demonstrates this approach.

### Example 11-14. Adding support for `-WhatIf` and `-Confirm`

```
function Invoke-MyAdvancedFunction
{
    [CmdletBinding(SupportsShouldProcess = $true)]
    param()

    if($psCmdlet.ShouldProcess("test.txt", "Remove Item"))
    {
        "Removing test.txt"
    }

    Write-Verbose "Verbose Message"
}
```

Now your advanced function is as well-behaved as built-in PowerShell cmdlets!

```
PS > Invoke-MyAdvancedFunction -WhatIf
What if: Performing operation "Remove Item" on Target "test.txt".
```

If your command causes a high-impact result that should be evaluated with caution, call the `$psCmdlet.ShouldContinue()` method. This generates a warning for users—but be sure to support a `-Force` parameter that lets them bypass this message.

```
function Invoke-MyDangerousFunction
{
    [CmdletBinding()]
    param(
        [Switch] $Force
    )

    if($Force -or $psCmdlet.ShouldContinue(
        "Do you wish to invoke this dangerous operation?
         Changes can not be undone.",
        "Invoke dangerous action?"))
    {
        "Invoking dangerous action"
    }
}
```

This generates a standard PowerShell confirmation message:

```
PS > Invoke-MyDangerousFunction

Invoke dangerous action?
Do you wish to invoke this dangerous operation? Changes can not be undone.
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Invoking dangerous action

PS > Invoke-MyDangerousFunction -Force
Invoking dangerous action
```

To explore the `$psCmdlet` automatic variable further, you can use [Example 11-15](#ch11.xhtml#invoke-advancedfunctionps1). This command creates the bare minimum of advanced functions, and then invokes whatever script block you supply within it.

### Example 11-15. Invoke-AdvancedFunction.ps1

```
param(
    [Parameter(Mandatory = $true)]
    [ScriptBlock] $Scriptblock
    )

## Invoke the script block supplied by the user.
& $scriptblock
```

For open-ended exploration, use `$host.EnterNestedPrompt()` as the script block:

```
PS > Invoke-AdvancedFunction { $host.EnterNestedPrompt() }
PS > $psCmdlet | Get-Member


   TypeName: System.Management.Automation.PSScriptCmdlet

Name                                MemberType Definition
----                                ---------- ----------
(...)
WriteDebug                          Method     System.Void WriteDebug(s...
WriteError                          Method     System.Void WriteError(S...
WriteObject                         Method     System.Void WriteObject(...
WriteProgress                       Method     System.Void WriteProgres...
WriteVerbose                        Method     System.Void WriteVerbose...
WriteWarning                        Method     System.Void WriteWarning...
(...)
ParameterSetName                    Property   System.String ParameterS...

(Now at a nested prompt)
PS >> exit

(Now back to the regular prompt)
PS >
```

For more about cmdlet support in the PowerShell engine, see the developer's reference in the [PowerShell Programmer's Guide](https://oreil.ly/jJrgp).

## See Also

[Recipe 11.11, "Access Arguments of a Script, Function, or Script Block"](#ch11.xhtml#access_arguments_of_a_script_function_or)

# 11.16 Add Help to Scripts or Functions

## Problem

You want to make your command and usage information available to the `Get-Help` command.

## Solution

Add descriptive help comments (with help-specific special tags like `.SYNOPSIS`, `.EXAMPLE`, and `.OUTPUTS`) at the beginning of your script for its synopsis, description, examples, notes, and more. Add descriptive help comments before parameters to describe their meaning and behavior:

```
##############################################################################
##
## Measure-CommandPerformance
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Measures the average time of a command, accounting for natural variability by
automatically ignoring the top and bottom 10%.

.EXAMPLE

PS > Measure-CommandPerformance.ps1 { Start-Sleep -m 300 }

Count    : 30
Average  : 312.10155
(...)

#>

param(
    ## The command to measure
    [Scriptblock] $Scriptblock,

    ## The number of times to measure the command's performance
    [int] $Iterations = 30
)

Set-StrictMode -Version 3

## Figure out how many extra iterations we need to account for the outliers
$buffer = [int] ($iterations * 0.1)
$totalIterations = $iterations + (2 * $buffer)

## Get the results
$results = 1..$totalIterations |
    Foreach-Object { Measure-Command $scriptblock }

## Sort the results, and skip the outliers
$middleResults = $results | Sort TotalMilliseconds |
    Select -Skip $buffer -First $iterations

## Show the average
$middleResults | Measure-Object -Average TotalMilliseconds
```

## Discussion

Like parameter validation, discussed in [Recipe 11.12](#ch11.xhtml#add_validation_to_parameters), rich help is something traditionally supported in only the most high-end commands. For most commands, you're lucky if you can figure out how to get some form of usage message.

As with PowerShell's easy-to-define support for advanced parameter validation, adding help to commands and functions is extremely simple. Despite its simplicity, comment-based help provides all the power you've come to expect of fully featured PowerShell commands: overview, description, examples, parameter-specific details, and more.

PowerShell creates help for your script or function by looking at its comments. If the comments include any supported help tags (like `.SYNOPSIS`, `.EXAMPLE`, `.OUTPUTS`), PowerShell adds those to the help for your command.

### Note

To speed up processing of these help comments, PowerShell places restrictions on where they may appear. In addition, if it encounters a comment that is *not* a help-based comment, it stops searching that block of comments for help tags. This may come as a surprise if you're used to placing headers or copyright information at the beginning of your script. The Solution demonstrates how to avoid this problem by putting the header and comment-based help in separate comment blocks. For more information about these guidelines, type `Get-Help about_Comment_Based_Help`.
