
[Recipe 13.6, “Write Culture-Aware Scripts”](#ch13.xhtml#write_culture-aware_scripts)

# 13.2 Read a Key of User Input

## Problem

You want your script to get a single keypress from the user.

## Solution

For most purposes, use the `[Console]::ReadKey()` method to read a key:

```
PS > $key = [Console]::ReadKey($true)
PS > $key

                 KeyChar                   Key              Modifiers
                 -------                   ---              ---------
                       h                     H                    Alt
```

For highly interactive use (for example, when you care about key down and key up), use:

```
PS > $key = $host.UI.RawUI.ReadKey("NoEcho,IncludeKeyDown")
PS > $key

    VirtualKeyCode          Character    ControlKeyState      KeyDown
    --------------          ---------    ---------------      -------
                16                    ...ssed, NumLockOn         True

PS > $key.ControlKeyState
ShiftPressed, NumLockOn
```

## Discussion

For most purposes, the `[Console]::ReadKey()` is the best way to get a keystroke from a user, as it accepts simple keypresses and more complex keypresses that might include the `Ctrl`, `Alt`, and `Shift` keys. We pass the `$true` parameter to tell the method to not display the character on the screen, and only to return it to us.

### Note

If you want to read a key of user input as a way to pause your script, you can use PowerShell’s built-in `pause` command.

If you need to capture individual key down and key up events (including those of the `Ctrl`, `Alt`, and `Shift` keys), use the `$host.UI.RawUI.ReadKey()` method.

# 13.3 Program: Display a Menu to the User

It is often useful to read input from the user but restrict input to a list of choices that you specify. The following script lets you access PowerShell’s prompting functionality in a manner that is friendlier than what PowerShell exposes by default. It returns a number that represents the position of the user’s choice from the list of options you provide.

PowerShell’s prompting requires that you include an accelerator key (the `&` before a letter in the option description) to define the keypress that represents that option. Since you don’t always control the list of options (for example, a list of possible directories), [Example 13-1](#ch13.xhtml#read-hostwithpromptps1) automatically generates sensible accelerator characters for any descriptions that lack them.

##### Example 13-1. Read-HostWithPrompt.ps1

```
#############################################################################
##
## Read-HostWithPrompt
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Read user input, with choices restricted to the list of options you
provide.

.EXAMPLE

PS > $caption = "Please specify a task"
PS > $message = "Specify a task to run"
PS > $option = "&Clean Temporary Files","&Defragment Hard Drive"
PS > $helptext = "Clean the temporary files from the computer",
>>              "Run the defragment task"
>>
PS > $default = 1
PS > Read-HostWithPrompt $caption $message $option $helptext $default

Please specify a task
Specify a task to run
[C] Clean Temporary Files  [D] Defragment Hard Drive  [?] Help
(default is "D"):?
C - Clean the temporary files from the computer
D - Run the defragment task
[C] Clean Temporary Files  [D] Defragment Hard Drive  [?] Help
(default is "D"):C
0

#>

param(
    ## The caption for the prompt
    $Caption = $null,

    ## The message to display in the prompt
    $Message = $null,

    ## Options to provide in the prompt
    [Parameter(Mandatory = $true)]
    $Option,

    ## Any help text to provide
    $HelpText = $null,

    ## The default choice
    $Default = 0
)

Set-StrictMode -Version 3

## Create the list of choices
$choices = New-Object `
    Collections.ObjectModel.Collection[Management.Automation.Host.ChoiceDescription]

## Go through each of the options, and add them to the choice collection
for($counter = 0; $counter -lt $option.Length; $counter++)
{
    $choice = New-Object Management.Automation.Host.ChoiceDescription `
        $option[$counter]

    if($helpText -and $helpText[$counter])
    {
        $choice.HelpMessage = $helpText[$counter]
    }

    $choices.Add($choice)
}

## Prompt for the choice, returning the item the user selected
$host.UI.PromptForChoice($caption, $message, $choices, $default)
```

For more information about running scripts, see [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools).

## See Also

[Recipe 1.2, “Run Programs, Scripts, and Existing Tools”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

# 13.4 Display Messages and Output to the User

## Problem

You want to display messages and other information to the user.

## Solution

Simply have your script output the string information. If you like to be more explicit in your scripting, call the `Write-Output` cmdlet:

```
PS > function Get-Information
{
    "Hello World"
    Write-Output (1 + 1)
}

PS > Get-Information
Hello World
2
PS > $result = Get-Information
PS > $result[1]
2
```

## Discussion

Most scripts that you write should output richly structured data, such as the actual count of bytes in a directory (if you’re writing a directory information script). That way, other scripts can use the output of that script as a building block for their functionality.

When you do want to provide output specifically to the user, use the `Write-Host`, `Write-Debug`, and `Write-Verbose` cmdlets:

```
PS > function Get-DirectorySize
{
    $size = (Get-ChildItem | Measure-Object -Sum Length).Sum
    Write-Host ("Directory size: {0:N0} bytes" -f $size)
}

PS > Get-DirectorySize
Directory size: 46,581 bytes
PS > $size = Get-DirectorySize
Directory size: 46,581 bytes
```

In addition to plain text, the `Write-Host` cmdlet lets you set output colors through its `-ForegroundColor` and `-BackgroundColor` parameters. For full control, you can even emit ANSI escape sequences directly, as shown in [Figure 13-1](#ch13.xhtml#example_write_host_output).

![](Images/wps4_1301.png)

If you want a message to help you (or the user) diagnose and debug your script, use the `Write-Debug` cmdlet. If you want a message to provide detailed trace-type output, use the `Write-Verbose` cmdlet, as shown in [Example 13-2](#ch13.xhtml#a_function_that_provides_debug_and_verbo).

##### Example 13-2. A function that provides debug and verbose output

```
PS > function Get-DirectorySize
{
    Write-Debug "Current Directory: $(Get-Location)"

    Write-Verbose "Getting size"
    $size = (Get-ChildItem | Measure-Object -Sum Length).Sum
    Write-Verbose "Got size: $size"

    Write-Host ("Directory size: {0:N0} bytes" -f $size)
}

PS > $DebugPreference = "Continue"
PS > Get-DirectorySize
DEBUG: Current Directory: D:\lee\OReilly\Scripts\Programs
Directory size: 46,581 bytes
PS > $DebugPreference = "SilentlyContinue"
PS > $VerbosePreference = "Continue"
PS > Get-DirectorySize
VERBOSE: Getting size
VERBOSE: Got size: 46581
Directory size: 46,581 bytes
PS > $VerbosePreference = "SilentlyContinue"
```

However, be aware that this type of output bypasses normal file redirection and is therefore difficult for the user to capture. In the case of the `Write-Host` cmdlet, use it only when your script already generates other structured data that the user would want to capture in a file or variable. For more information about capturing `Write-Host` output, see [“Capturing Output”](#app01.xhtml#capturing_output).

Most script authors eventually run into the problem illustrated by [Example 13-3](#ch13.xhtml#an_error_message_caused_by_formatting_st) when their script tries to output formatted data to the user.

##### Example 13-3. An error message caused by formatting statements
