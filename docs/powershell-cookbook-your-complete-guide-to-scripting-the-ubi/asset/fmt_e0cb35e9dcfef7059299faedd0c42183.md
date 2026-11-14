
```
PS > cmd /c echo 'quotes' "and" $variables @{ etc = $true }
quotes and System.Collections.Hashtable

PS > cmd --% /c echo 'quotes' "and" $variables @{ etc = $true }
'quotes' "and" $variables @{ etc = $true }
```

## Discussion

One of PowerShell’s primary goals has always been command consistency. Because of this, cmdlets are very regular in the way that they accept parameters. Native executables write their own parameter parsing, so you never know what to expect when working with them. In addition, PowerShell offers many features that make you more efficient at the command line: command substitution, variable expansion, and more. Since many native executables were written before PowerShell was developed, they may use special characters that conflict with these features.

As an example, the command given in the Solution uses all the special characters available on a typical keyboard. Without the quotes, PowerShell treats some of them as language features, as shown in [`#ch01.xhtml#sample_of_special`](#ch01.xhtml#sample_of_special).

Table 1-1. Sample of special characters

| Special character | Meaning |
| --- | --- |
| `"` | The beginning (or end) of quoted text |
| `#` | The beginning of a comment |
| `$` | The beginning of a variable |
| `&` | The background pipeline operator |
| `( )` | Parentheses used for subexpressions |
| `;` | Statement separator |
| `{ }` | Script block |
| `|` | Pipeline separator |
| `` ` `` | Escape character |

When surrounded by single quotes, PowerShell accepts these characters as written, without the special meaning.

Despite these precautions, you may still sometimes run into a command that doesn’t seem to work when called from PowerShell. For the most part, you can resolve these with the verbatim argument marker (`--%`) that prevents PowerShell from interpreting any of the remaining characters on the line. You can place this marker anywhere in the command’s arguments, letting you benefit from PowerShell constructs where appropriate. The following example uses a PowerShell variable for some of the command arguments, but then uses verbatim arguments for the rest:

```
PS > $username = "Lee"
PS > cmd /c echo Hello $username with 'quotes' "and" $variables @{ etc = $true }
Hello Lee with quotes and System.Collections.Hashtable
PS > cmd /c echo Hello $username `
     --% with 'quotes' "and" $variables @{ etc = $true }
Hello Lee with 'quotes' "and" $variables @{ etc = $true }
```

While in this mode, PowerShell also accepts `cmd.exe`-style environment variables—as these are frequently used in commands that “just used to work”:

```
PS > $env:host = "localhost"
PS > ping %host%
Ping request could not find host %host%. Please check the name and try again.
PS > ping --% %host%

Pinging localhost [127.0.1.1] with 32 bytes of data:
(...)
```

## See Also

- [Appendix A, *PowerShell Language and Environment*](#app01.xhtml#powershell_language_and_environment)

# 1.5 Supply Default Values for Parameters

## Problem

You want to define a default value for a parameter in a PowerShell command.

## Solution

Add an entry to the `PSDefaultParameterValues` hashtable:

```
PS > Get-Process

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    150      13     9692       9612    39    21.43    996 audiodg
   1013      84    45572      42716   315     1.67   4596 WWAHost
(...)

PS > $PSDefaultParameterValues["Get-Process:ID"] = $pid
PS > Get-Process

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    584      62   132776     157940   985    13.15   9104 powershell

PS > Get-Process -Id 0

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
      0       0        0         20     0               0 Idle
```

## Discussion

In PowerShell, many commands (cmdlets and advanced functions) have parameters that let you configure their behavior. For a full description of how to provide input to commands, see [“Running Commands”](#app01.xhtml#running_scripts). Sometimes, though, supplying values for those parameters at each invocation becomes awkward or repetitive.

In early versions of PowerShell, it was the responsibility of each cmdlet author to recognize awkward or repetitive configuration properties and build support for “preference variables” into the cmdlet itself. For example, the `Send-MailMessage` cmdlet looks for the `$PSEmailServer` variable if you do not supply a value for its `-SmtpServer` parameter.

To make this support more consistent and configurable, PowerShell supports the `PSDefaultParameterValues` preference variable. This preference variable is a hashtable. Like all other PowerShell hashtables, entries come in two parts: the key and the value.

Keys in the `PSDefaultParameterValues` hashtable must match the pattern `cmdlet`:`parameter`—that is, a cmdlet name and parameter name, separated by a colon. Either (or both) may use wildcards, and spaces between the command name, colon, and parameter are ignored.

Values for the cmdlet/parameter pairs can be either a simple parameter value (a string, boolean value, integer, etc.) or a script block. Simple parameter values are what you will use most often.

If you need the default value to dynamically change based on what parameter values are provided so far, you can use a script block as the default. When you do so, PowerShell evaluates the script block and uses its result as the default value. If your script block doesn’t return a result, PowerShell doesn’t apply a default value.

When PowerShell invokes your script block, `$args[0]` contains information about any parameters bound so far: `BoundDefaultParameters`, `BoundParameters`, and `BoundPositionalParameters`. As one example of this, consider providing default values to the `-Credential` parameter based on the computer being connected to. Here’s a function that simply outputs the credential being used:

```
function RemoteConnector
{
    param(
        [Parameter()]
        $ComputerName,

        [Parameter(Mandatory = $true)]
        $Credential)

    "Connecting as " + $Credential.UserName
}
```

Now, you can define a credential map:

```
PS > $credmap = @{}
PS > $credmap["RemoteComputer1"] = Get-Credential
PS > $credmap["RemoteComputer2"] = Get-Credential
```

Then, create a parameter default for all `Credential` parameters that looks at the `ComputerName` bound parameter:

```
$PSDefaultParameterValues["*:Credential"] = {
    if($args[0].BoundParameters -contains "ComputerName")
    {
        $cred = $credmap[$PSBoundParameters["ComputerName"]]
        if($cred) { $cred }
    }
}
```

Here is an example of this in use:

```
PS > RemoteConnector -ComputerName RemoteComputer1
Connecting as UserForRemoteComputer1
PS > RemoteConnector -ComputerName RemoteComputer2
Connecting as UserForRemoteComputer2
PS > RemoteConnector -ComputerName RemoteComputer3

cmdlet RemoteConnector at command pipeline position 1
Supply values for the following parameters:
Credential: (...)
```

For more information about working with hashtables in PowerShell, see [“Hashtables (Associative Arrays)”](#app01.xhtml#hashtables_associative_arrays).

## See Also

- [“Hashtables (Associative Arrays)”](#app01.xhtml#hashtables_associative_arrays)
- [“Running Commands”](#app01.xhtml#running_scripts)

# 1.6 Invoke a Long-Running or Background Command

## Problem

You want to invoke a long-running command on a local or remote computer.

## Solution

Invoke the command as a `Job` to have PowerShell run it in the background:
