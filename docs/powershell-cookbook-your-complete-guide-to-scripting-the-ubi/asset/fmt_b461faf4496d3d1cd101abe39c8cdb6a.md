
# 29.16 Pass Local Variables to Remote Sessions

The solution gives an example of this approach. When you supply `$myNumber` by itself, that refers to `$myNumber` within the context of the remote session that `$s` represents. If you add the `$USING:` prefix, PowerShell takes the value of that variable from your local session—the one that created and controls `$s`.

The `$USING:` prefix was added in PowerShell version 3, and works even when connecting to a machine that supports only PowerShell version 2.

In some scenarios, though, you might need to pass data in a way other than through the `$USING:` prefix. [Example 29-5](#ch29.xhtml#passing_argumentlist_values) demonstrates how to solve this problem. On a client computer, we request data (for example, a credential) from the user. We make a connection to `RemoteComputer` using that credential and invoke a command. In this example, the command itself makes yet another connection—this time to `leeholmes1c23`. That final command simply retrieves the computer name of the remote system. Rather than hardcode a username and password (or request them again), it uses the `$cred` variable passed in to the original call to `Invoke-Command`.

## Example 29-5. Passing Values Through the ArgumentList Parameter

```
PS > $cred = Get-Credential

PS > $command = {
    param($cred)

    Invoke-Command leeholmes1c23 {
        "Hello from $($env:Computername)" } -Credential $cred
}

PS > Invoke-Command RemoteComputer $command -ArgumentList $cred -Credential $cred
Hello from LEEHOLMES1C23
```

To support this, the `Invoke-Command` cmdlet offers the `-ArgumentList` parameter. Variables supplied to this parameter will be converted into a version safe for remoting, which will then be made available to the commands inside of the `-ScriptBlock` parameter.

### Note

Arguments that you supply to the `-ArgumentList` parameter go through a serialization process before being sent to the remote computer. Although their properties closely resemble the original objects, they no longer have methods. For more information about PowerShell serialization, see [Recipe 29.5](#ch29.xhtml#invoke_command_another_computer).

As with arguments in other scripts, functions, and script blocks, the script block used in `Invoke-Command` can access arguments directly through the `$args` array, or through a `param()` statement to make the script easier to read. Unlike most `param()` statements, however, these parameter statements must all be positional. Named arguments (e.g., `-ArgumentList "-Cred","$cred"`) are not supported, nor are advanced parameter attributes (such as `[Parameter(Mandatory = $true)]`).

For more information about arguments and `param()` statements, see [Recipe 11.11](#ch11.xhtml#access_arguments_of_a_script_function_or).

## See Also

- [Recipe 11.11, “Access Arguments of a Script, Function, or Script Block”](#ch11.xhtml#access_arguments_of_a_script_function_or)
- [Recipe 29.5, “Invoke a Command on a Remote Computer”](#ch29.xhtml#invoke_command_another_computer)

# 29.17 Manage and Edit Files on Remote Machines

## Problem

You have a remote machine and want to send and receive files from it. Or, you want to edit a file on the remote machine directly.

## Solution

Use the `Copy-Item -ToSession` and `-FromSession` parameters to move files between machines over an active PowerShell Remoting session:

```
PS > "Hello World" > myfile.txt
PS > $s = New-PSSession -Computername lee-desk
PS > Copy-Item myfile.txt -ToSession $s -Destination c:\temp\copied.txt
PS > Invoke-Command $s { Get-Content c:\temp\copied.txt }
Hello World
PS > Invoke-Command $s { Set-Content c:\temp\copied.txt "Heya!" }
PS > Copy-Item -FromSession $s -Path c:\temp\copied.txt -Destination newfile.txt
PS > Get-Content newfile.txt
Heya!
```

In the PowerShell ISE or Visual Studio Code, use the `psedit` command to edit a file on a remote machine directly:

```
====> PowerShell Integrated Console v2020.6.0 <=====

PS > $s = New-PSSession localhost
PS > "Hello World" > c:\temp\hello.txt
PS > Enter-PSSession $s

[localhost]: PS > psedit c:\temp\hello.txt

(... edit the file in the window that pops up ...)

[localhost]: PS > exit

PS > Get-Content c:\temp\hello.txt

Back at you!
```

## Discussion

When you’re working with remote computers, a common problem you’ll face is how to bring your local tools and environment to that computer. Using file shares or FTP transfers is a common way to share tools between systems, but these options aren’t always available. Especially if you’ve been careful about securing them, they likely don’t have SMB or the remote administrative share enabled.

Often, PowerShell Remoting is the only avenue you have to a machine.

To solve these problems, the `Copy-Item` cmdlet offers both the `-FromSession` and `-ToSession` parameters. When you supply one of these parameters with an active PowerShell Remoting session, PowerShell will automatically transfer files back and forth on your behalf.

### Note

This capability is disabled by default when you connect to Just Enough Administration task-specific endpoints. To enable this functionality safely, your endpoint will need to enable the User Drive feature. For more information about Just Enough Administration, see [Recipe 18.18](#ch18.xhtml#create_task_specific_endpoint).

As demonstrated by the Solution, the `psedit` command is another way to quickly edit scripts and files on a remote machine. This command requires that you’ve entered an interactive session with either the PowerShell ISE or Visual Studio Code. When you run this command, these hosts bring a copy of the remote file back to your computer. Edits that you make to your local copy then get transparently sent back to the remote computer on your behalf.

Visual Studio Code extends this remote editing experience even further by supporting it during interactive debugging sessions. If a script on a remote machine hits a debugging breakpoint while you’re connected, Visual Studio Code will automatically open a copy of the target script in the UI to let you interactively debug and edit that script. To see an example of this in action, see [Recipe 14.8](#ch14.xhtml#debug_script_remote_machine).

## See Also

- [Recipe 14.8, “Debug a Script on a Remote Machine”](#ch14.xhtml#debug_script_remote_machine)
- [Recipe 18.18, “Create a Task-Specific Remoting Endpoint”](#ch18.xhtml#create_task_specific_endpoint)

# 29.18 Configure Advanced Remoting Quotas and Options

## Problem

You want to configure compression, profiles, proxy authentication, certificate verification, or culture information for a remote session.

## Solution

For client-side configuration settings, call the `New-PSSessionOption` cmdlet and provide values for parameters that you want to customize:

```
PS > $options = New-PSSessionOption -Culture "fr-CA"
PS > $sess = New-PSSession Lee-Desk -Cred Lee -SessionOption $options
PS > Invoke-Command $sess { Get-Date | Out-String }

20 février 2010 17:40:16
```

For server-side configuration settings, review the options under `WSMan:\localhost\Shell` and `WSMan:\localhost\Service`:

```
Set-Item WSMan:\localhost\shell\MaxShellsPerUser 10
```

## Discussion

PowerShell lets you define advanced client connection options through two paths: the `New-PSSessionOption` cmdlet and the `$PSSessionOption` automatic variable.

When you call the `New-PSSession` cmdlet, PowerShell returns an object that holds configuration settings for a remote session. You can customize all of the values through the cmdlet’s parameters or set properties on the object that is returned.

### Note

Several of the options refer to timeout values: `OperationTimeout`, `OpenTimeout`, `CancelTimeout`, and `IdleTimeout`. These parameters are generally not required (for example, even when invoking a long-running command), but they can be used to overcome errors when you encounter extremely slow or congested network conditions.
