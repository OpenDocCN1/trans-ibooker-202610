
## Discussion

With many remote management technologies, a common problem is that closing your local instance automatically closes any remote shells you're connected to. If your remote shell had valuable information in it or was running jobs, this information was lost.

To resolve this issue, PowerShell supports the `Disconnect-PSSession` and `Connect-PSSession` cmdlets. These let you disconnect and connect to remote sessions, respectively.

When you want to discover disconnected sessions on a remote computer, use the `-ComputerName` parameter of the `Get-PSSession` cmdlet to retrieve them.

In addition to disconnecting specific sessions, you can invoke a long-running command on a computer and immediately disconnect from it. For this purpose, the `Invoke-Command` cmdlet offers the `-InDisconnectedSession` parameter:

```
PS > $s = New-PSSession RemoteComputer -Name ConnectTest
PS > Invoke-Command $s { $saved = "Hello World" }
PS > Disconnect-PSSession $s

 Id Name            ComputerName    State         ConfigurationName
 -- ----            ------------    -----         -----------------
  7 ConnectTest     RemoteComputer  Disconnected  Microsoft.PowerShell

## From potentially another shell or computer
PS > $s2 = Get-PSSession -ComputerName RemoteComputer -Name ConnectTest
PS > Connect-PSSession $s2

 Id Name            ComputerName    State         ConfigurationName
 -- ----            ------------    -----         -----------------
  7 ConnectTest     RemoteComputer  Disconnected  Microsoft.PowerShell

PS > Invoke-Command $s2 { $saved }
Hello World
```

```
PS > Invoke-Command RemoteComputer {
    1..10 | % { Start-Sleep -Seconds 10; $_ } } -InDisconnectedSession

 Id Name            ComputerName    State         ConfigurationName
 -- ----            ------------    -----         -----------------
  3 Session2        RemoteComputer  Disconnected  Microsoft.PowerShell

PS > $s = Get-PSSession -Computername RemoteComputer -Name Session2

PS > Connect-PSSession $s

 Id Name            ComputerName    State         ConfigurationName
 -- ----            ------------    -----         -----------------
  3 Session2        RemoteComputer  Opened        Microsoft.PowerShell

PS > Receive-PSSession $s
1
2
3
(...)
```

When you use the `-InDisconnectedSession` parameter, the command you run is likely to generate output. If it generates more than a megabyte of output while disconnected, PowerShell pauses your command until you connect and call the `Receive-PSSession` command to retrieve it. When calculating this megabyte of output, PowerShell considers only output that would normally have been sent directly to the connected client—output that you would typically see on your screen. Output stored in variables doesn't count toward this limit, nor does output that you redirect to files.

If you'd like PowerShell to keep running your command no matter how much output it generates, you can use the `OutputBufferingMode` property of the `-SessionOption` parameter:

```
Invoke-Command RemoteComputer {
    ... commands ...
} -InDisconnectedSession -SessionOption @{ OutputBufferingMode = "Drop" }
```

When you specify `Drop` as the `OutputBufferingMode`, PowerShell retains only the last megabyte of output.

## See Also

[Recipe 29.4, "Interactively Manage a Remote Computer"](#ch29.xhtml#interactively_manage_remote_computer)

[Recipe 29.5, "Invoke a Command on a Remote Computer"](#ch29.xhtml#invoke_command_another_computer)

# 29.7 Program: Remotely Enable PowerShell Remoting

Although not required for Windows Server by default, you may sometimes need to remotely enable PowerShell Remoting. Unfortunately, most machines are not configured to support remote task management. Most are, however, configured to support WMI connections. As a bootstrapping step, we can use the `Create()` method of the `Win32_Process` class to launch an instance of PowerShell, and then provide PowerShell with the commands to enable PowerShell Remoting.

The script shown in [Example 29-1](#ch29.xhtml#enable-remotepsremotingps1) automates this cumbersome process.

### Example 29-1. Enable-RemotePSRemoting.ps1

```
##############################################################################
##
## Enable-RemotePsRemoting
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Enables PowerShell Remoting on a remote computer. Requires that the machine
responds to WMI requests, and that its operating system is Windows Vista or
later.

.EXAMPLE

PS > Enable-RemotePsRemoting <Computer>

#>

param(
    ## The computer on which to enable remoting
    $Computername,

    [Switch] $SkipNetworkProfileCheck,

    ## The credential to use when connecting
    [PSCredential] $Credential
)

Set-StrictMode -Version 3

$VerbosePreference = "Continue"

Write-Verbose "Configuring $computername"
$skipNetworkProfileCheckFlag = '$' + $SkipNetworkProfileCheck.IsPresent
$command = "powershell -NoProfile -Command" +
    "Enable-PSRemoting -SkipNetworkProfileCheck:$skipNetworkProfileCheckFlag -Force"

if($Credential)
{
    $null = Invoke-WmiMethod -Computer $computername -Credential $credential `
        Win32_Process Create -Args $command

    Start-Sleep -Seconds 10

    Write-Verbose "Testing connection"
    Invoke-Command $computername {
        Get-WmiObject Win32_ComputerSystem } -Credential $credential
}
else {
    $null = Invoke-WmiMethod -Computer $computername Win32_Process Create -Args $command
    Start-Sleep -Seconds 10

    Write-Verbose "Testing connection"
    Invoke-Command $computername { Get-WmiObject Win32_ComputerSystem }
}
```

For more information about running scripts, see [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools).

## See Also

[Recipe 1.2, "Run Programs, Scripts, and Existing Tools"](#ch01.xhtml#run_programs_scripts_and_existing_tools)

[Recipe 28.1, "Access Windows Management Instrumentation and CIM Data"](#ch28.xhtml#access_windows_management_instrumentatio)

[Recipe 29.2, "Enable PowerShell Remoting on a Computer"](#ch29.xhtml#enable_powershell_remoting)

# 29.8 Program: Invoke a PowerShell Expression on a Remote Machine

PowerShell includes fantastic support for command execution on remote machines through its PowerShell Remoting features. These require only that the remote system have PowerShell available and have Remoting enabled.

If PowerShell Remoting is not available on a remote machine, many commands support their own remoting over WMI or DCOM. These do not require PowerShell Remoting to be configured on the remote system, but do require the specific protocol (WMI or DCOM) to be enabled.

If none of these prerequisites is possible, [Example 29-3](#ch29.xhtml#invoke-remoteexpressionps1) offers an alternative. It uses [PsExec](https://oreil.ly/QVeM9) to support the actual remote command execution.

This script offers more power than just remote command execution, however. As [Example 29-2](#ch29.xhtml#invoking_a_powershell_expression_on_a_re) demonstrates, it leverages PowerShell's capability to import and export strongly structured data, so you can work with the command output using many of the same techniques you use to work with command output on the local system. It demonstrates this power by filtering command output on the remote system but sorting it on the local system.

### Example 29-2. Invoking a PowerShell expression on a remote machine
