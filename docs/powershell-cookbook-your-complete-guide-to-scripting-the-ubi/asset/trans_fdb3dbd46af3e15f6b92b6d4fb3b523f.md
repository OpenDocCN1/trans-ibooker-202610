
# 29.9 测试两台计算机之间的连接性

## 示例 29-3：Invoke-RemoteExpression.ps1

### 使用示例

```
PS > $command = { Get-Process | Where-Object { $_.Handles -gt 1000 } }
PS > Invoke-RemoteExpression \\LEE-DESK $command | Sort-Object Handles
Handles  NPM(K)  PM(K)  WS(K)  VM(M)  CPU(s)    Id ProcessName
-------  ------  -----  -----  -----  ------    -- -----------
   1025       8   3780   3772     32  134.42   848 csrss
   1306      37  50364  64160    322  409.23  4012 OUTLOOK
   1813      39  54764  36360    321  340.45  1452 iTunes
   2316     273  29168  41164    218  134.09  1244 svchost
```

由于这些高度结构化的数据来自另一台系统上的对象，`PowerShell` 不会重新生成这些对象的功能（极少数情况除外）。有关导入和导出结构化数据的详细信息，请参阅 [Recipe 10.5](#ch10.xhtml#easily_import_and_export_your_structured)。

### 脚本实现

```
##############################################################################
##
## Invoke-RemoteExpression
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Invoke a PowerShell expression on a remote machine. Requires `PsExec` from
`http://live.sysinternals.com/tools/psexec.exe`. If the remote machine
supports PowerShell Remoting, use that instead!

.EXAMPLE

PS > Invoke-RemoteExpression LEE-DESK { Get-Process }
Retrieves the output of a simple command from a remote machine

.EXAMPLE

PS > (Invoke-RemoteExpression LEE-DESK { Get-Date }).AddDays(1)
Invokes a command on a remote machine. Since the command returns one of
PowerShell's primitive types (a `DateTime` object,) you can manipulate
its output as an object afterward.

.EXAMPLE

PS > Invoke-RemoteExpression LEE-DESK { Get-Process } | Sort Handles
Invokes a command on a remote machine. The command does not return one of
PowerShell's primitive types, but you can still use PowerShell's filtering
cmdlets to work with its structured output.

#>

param(
    ## The computer on which to invoke the command.
    $ComputerName = "$ENV:ComputerName",

    ## The scriptblock to invoke on the remote machine.
    [Parameter(Mandatory = $true)]
    [ScriptBlock] $ScriptBlock,

    ## The username / password to use in the connection
    $Credential,

    ## Determines if PowerShell should load the user's PowerShell profile
    ## when invoking the command.
    [switch] $NoProfile
)

Set-StrictMode -Version 3

## Prepare the computername for PSExec
if($ComputerName -notmatch '^\\')
{
    $ComputerName = "\\$ComputerName"
}

## Prepare the command line for PsExec. We use the XML output encoding so
## that PowerShell can convert the output back into structured objects.
## PowerShell expects that you pass it some input when being run by PsExec
## this way, so the 'echo .' statement satisfies that appetite.
$commandLine = "echo . | powershell -Output XML "

if($noProfile)
{
    $commandLine += "-NoProfile "
}

## Convert the command into an encoded command for PowerShell
$commandBytes = [System.Text.Encoding]::Unicode.GetBytes($scriptblock)
$encodedCommand = [Convert]::ToBase64String($commandBytes)
$commandLine += "-EncodedCommand $encodedCommand"

## Collect the output and error output
$errorOutput = [IO.Path]::GetTempFileName()

if($Credential)
{
    ## This lets users pass either a username, or full credential to our
    ## credential parameter
    $credential = Get-Credential $credential
    $networkCredential = $credential.GetNetworkCredential()
    $username = $networkCredential.Username
    $password = $networkCredential.Password

    $output = psexec $computername /user $username /password $password `
        /accepteula cmd /c $commandLine 2>$errorOutput
}
else
{
    $output = psexec /acceptEula $computername cmd /c $commandLine 2>$errorOutput
}

## Check for any errors
$errorContent = Get-Content $errorOutput
Remove-Item $errorOutput

if($lastExitCode -ne 0)
{
    $OFS = "`n"
    $errorMessage = "Could not execute remote expression. "
    $errorMessage += "Ensure that your account has administrative " +
        "privileges on the target machine.`n"
    $errorMessage += ($errorContent -match "psexec.exe :")

    Write-Error $errorMessage
}

## Return the output to the user
$output
```

有关运行脚本的详细信息，请参阅 [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

## 另请参阅

[Recipe 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

[Recipe 10.5，“轻松导入和导出结构化数据”](#ch10.xhtml#easily_import_and_export_your_structured)

[Recipe 29.1，“查找支持自身远程处理的命令”](#ch29.xhtml#find_commands_that_support_remoting)

## 问题

你想要确定一台计算机的网络可用性，或两台计算机之间的网络可用性。

## 解决方案

使用 `Test-Connection` cmdlet 执行传统的网络 ping：

```
PS > Test-Connection leeholmes.com

Source    Destination     IPV4Address      IPV6Address
------    -----------     -----------      -----------
LEE-DESK  leeholmes.com   66.186.25.131    {}
LEE-DESK  leeholmes.com   66.186.25.131    {}
LEE-DESK  leeholmes.com   66.186.25.131    {}
LEE-DESK  leeholmes.com   66.186.25.131    {}
```

或者，`ping.exe` 实用程序仍然可用：

```
PS > ping leeholmes.com

Pinging leeholmes.com [66.186.25.131] with 32 bytes of data:
Reply from 66.186.25.131: bytes=32 time=38ms TTL=115
Reply from 66.186.25.131: bytes=32 time=36ms TTL=115
Reply from 66.186.25.131: bytes=32 time=37ms TTL=115
Reply from 66.186.25.131: bytes=32 time=41ms TTL=115

Ping statistics for 66.186.25.131:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 36ms, Maximum = 41ms, Average = 38ms
```

## 讨论

作为命令行 Shell，`PowerShell` 当然继续支持传统的命令行实用程序。最常见的网络诊断工具之一是 `ping.exe`，它在 `PowerShell` 中按预期工作。

`Test-Connection` cmdlet 提供与 `ping.exe` 相同的功能，外加大量额外功能。大多数 ping 实用程序只能验证当前计算机与目标计算机之间的连接，但 `Test-Connection` cmdlet 还允许你为网络测试指定*源计算机*。

`Test-Connection` cmdlet 或许最大的优势在于其基于对象的输出——这使得筛选、排序和分析变得容易得多。例如，一个用于监控一组域名平均响应时间的简单脚本：

```
$topTen = "google.com","facebook.com","youtube.com","yahoo.com",
    "live.com","wikipedia.org","blogger.com","baidu.com","msn.com",
    "qq.com"

## Test all of the connections, grouping by destination
$results = Test-Connection $topTen -ErrorAction SilentlyContinue |
    Group Destination

## Go through each of the addresses
$averages = foreach($group in $results)
{
    ## Figure out the average response time
    $averageResponse = $group.Group |
        Measure-Object -Average Latency | Select -Expand Average

    ## Create a new custom object to output the Address and ResponseTime
    [PSCustomObject] @{
        Address = $group.Name;
        ResponseTime = $averageResponse
    }
}

## Output the results
$averages | Sort-Object ResponseTime | Select Address,ResponseTime
```

该脚本产生以下输出：

```
Destination   ResponseTime
-----------   ------------
google.com              22
blogger.com           22.5
facebook.com         35.25
yahoo.com             37.5
youtube.com          86.25
wikipedia.org           99
baidu.com           203.25
qq.com              259.25
```
