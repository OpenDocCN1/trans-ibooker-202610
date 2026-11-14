
For more information about enabling PowerShell Remoting, see [Recipe 29.2](#ch29.xhtml#enable_powershell_remoting).

## See Also

[Recipe 29.2](#ch29.xhtml#enable_powershell_remoting)

# 29.2 Enable PowerShell Remoting on a Computer

## Problem

You want to allow remote management of a computer via PowerShell Remoting.

## Solution

Use the `Enable-PSRemoting` cmdlet to enable PowerShell Remoting:

```
PS > Enable-PSRemoting

WinRM Quick Configuration
Running command "Set-WSManQuickConfig" to enable this machine for remote
management through WinRM service.
 This includes:
    1. Starting or restarting (if already started) the WinRM service
    2. Setting the WinRM service type to autostart
    3. Creating a listener to accept requests on any IP address
    4. Enabling firewall exception for WS-Management traffic (for http only).

Do you want to continue?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help
(default is "Y"): Y

WinRM has been updated to receive requests.
WinRM service type changed successfully.
WinRM service started.
Configured LocalAccountTokenFilterPolicy to grant administrative rights
remotely to local users.

WinRM has been updated for remote management.
Created a WinRM listener on HTTP://* to accept WS-Man requests to any IP on
this machine.
WinRM firewall exception enabled.
```

## Discussion

As mentioned in this chapter’s introduction, PowerShell enables remoting by default for typical management scenarios: servers within a trusted or domain network and within the local subnet. On client machines or untrusted networks, PowerShell does not listen to network connections by default and it must be explicitly enabled.

PowerShell Remoting does not require any specific configuration to let you connect to a remote computer, but it does require a configuration step to allow connections from remote computers.

### Enable remoting on a single local machine

Once you’ve decided to enable remoting, PowerShell makes this a snap (after informing you of the impact). Simply call `Enable-PSRemoting` from an elevated shell. The solution demonstrates this approach. To bypass any user prompts or confirmation, also specify the `-Force` flag.

By default, `Enable-PSRemoting` is blocked on networks identified as public networks. If you generally trust the network you’re connected to, use the `Set-NetConnectionProfile` cmdlet to change the network profile to private:

```
Set-NetConnectionProfile -NetworkCategory Private
```

If you don’t generally trust the network you’re connected to but want to enable PowerShell Remoting, use the `-SkipNetworkProfileCheck` parameter.

As part of the `Enable-PSRemoting` process, PowerShell connects to the local WS-Management service to create and configure the PowerShell Remoting endpoint. This is done through a local network connection, so it’s impacted by the Windows restrictions on network connections. For example, Windows does not allow network connections to any account that has a blank password. If your administrator account has a blank password, PowerShell will be unable to properly create and configure the WSMan endpoint.

### Enable remoting on a remote machine

Remotely enabling PowerShell Remoting offers many unique challenges. Although you can certainly use Remote Desktop to connect to the system (and then essentially enable it locally), Remote Desktop does not lend itself to automation.

Instead, you can leverage another remoting technology that does lend itself to automation: Windows Management Instrumentation (WMI). WMI is enabled on most domain machines, but it offers only a minor facility for remote command execution: the `Create()` method of the `Win32_Process` WMI class. For more information about this approach, see [Recipe 29.7](#ch29.xhtml#program_remotely_enable_remoting).

### Enable remoting in an enterprise

If you want to enable PowerShell Remoting in an enterprise, Group Policy is the most flexible and scalable option. Through Group Policy settings, you can enable automatic configuration of WinRM endpoints and firewall rules. For more information about this approach, type `Get-Help about_remote_troubleshooting`.

## See Also

[Recipe 29.7, “Program: Remotely Enable PowerShell Remoting”](#ch29.xhtml#program_remotely_enable_remoting)

# 29.3 Enable SSH as a PowerShell Remoting Transport

## Problem

You want to use PowerShell Remoting to connect to remote Linux and macOS machines.

## Solution

Edit `/etc/ssh/sshd_config` (Linux) or `/private/etc/ssh/sshd_config` (macOS) on the machine you want to connect to and configure `/usr/bin/pwsh` (Linux) or `/usr/local/bin/pwsh` (macOS) as an SSH subsystem:

```
(...)
Subsystem sftp /usr/lib/openssh/sftp-server
Subsystem powershell /usr/bin/pwsh -sshs -NoLogo
```

Then restart sshd:

```
sudo service ssh restart
```

## Discussion

As we’ve discussed throughout this chapter, PowerShell Remoting is an incredibly versatile technology. It forms the rich communication channel for parallel script blocks, thread jobs, background jobs, cross-process debugging, remote debugging, Hyper-V console remoting, Container connections, and of course both interactive and fan-out remoting for Windows computers.

In addition to all of these options, you can also configure PowerShell to use SSH as a transport mechanism for PowerShell Remoting connections. If you’ve installed PowerShell Core on a remote machine, you can simply edit that machine’s SSH configuration to enable PowerShell as a recognized subsystem. After you’ve done that, interacting with a Linux or macOS system provides powerful and rich object-based management functionality that SSH itself could never have offered:

```
PS > $s = New-PSSession 172.27.124.129 -SSHTransport
lee@172.27.124.129's password: ***************************
PS > Invoke-Command $s { $PSVersionTable } -OutVariable versionTable

Name                           Value
----                           -----
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0…}
GitCommitId                    7.1.0
Platform                       Unix
WSManStackVersion              3.0
PSVersion                      7.1.0
SerializationVersion           1.1.0.1
PSEdition                      Core
OS                             Linux 5.4.0-48-generic #52-Ubuntu SMP
PSRemotingProtocolVersion      2.3

PS > $versionTable.OS
Linux 5.4.0-48-generic #52-Ubuntu SMP Thu Sep 10 10:58:49 UTC 2020
```

## See Also

[Recipe 1.1, “Install PowerShell Core”](#ch01.xhtml#install_powershell_core)

# 29.4 Interactively Manage a Remote Computer

## Problem

You want to interactively work with a remote computer as though it were a local PowerShell session.

## Solution

Use the `Enter-PSSession` cmdlet to connect to a remote session and manage it interactively:

```
PS > Enter-PSSession Lee-Desk
[lee-desk]: PS E:\Lee> Get-Process -Name PowerShell

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
   2834      14    85500      86256   218 ...22.83   8396 powershell
    421      12    39220      54204   189     7.41   9708 powershell

[lee-desk]: PS E:\Lee> exit
PS >
```

If your current account doesn’t have access to the remote computer, you can use the `-Credential` parameter to supply alternate credentials:

```
PS > $cred = Get-Credential LEE-DESK\Lee
PS > Enter-PSSession Lee-Desk -Cred $cred
```

## Discussion

Like many traditional shells, PowerShell Remoting offers a simple, direct, interactive management experience known simply as *interactive remoting*. You can use interactive remoting with remote computers, Hyper-V virtual machines, containers, and even Linux and macOS machines over SSH.

Just as in your local PowerShell sessions, you type commands and see their output. This remote PowerShell is just as powerful as your local one; all of the filtering, pipelining, and integrated language features continue to work.
