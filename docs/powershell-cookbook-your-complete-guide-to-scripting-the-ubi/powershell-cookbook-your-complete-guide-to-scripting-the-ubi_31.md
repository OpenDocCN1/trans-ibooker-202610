# 第 25 章 系统服务

## 25.0 简介

作为 Windows 上许多管理任务的支持机制，管理和使用系统服务自然融入了管理员的工具箱。

PowerShell 提供了一组 cmdlet 来帮助使系统服务的使用更加容易：从列出服务到生命周期管理，甚至服务安装。

## 25.1 列出所有正在运行的服务

### 问题

您想查看系统上正在运行的服务。

### 解决方案

要列出所有正在运行的服务，请使用 `Get-Service` cmdlet：

```
PS > Get-Service

Status   Name                DisplayName
------   ----                -----------
Running  ADAM_Test           Test
Stopped  Alerter             Alerter
Running  ALG                 Application Layer Gateway Service
Stopped  AppMgmt             Application Management
Stopped  aspnet_state        ASP.NET State Service
Running  AudioSrv            Windows Audio
Running  BITS                Background Intelligent Transfer Ser...
Running  Browser             Computer Browser
(...)
```

### 讨论

`Get-Service` cmdlet 检索系统上所有服务的信息。由于这些是丰富的 .NET 对象（类型为 `System.ServiceProcess.ServiceController`），您可以应用高级筛选和操作，使服务管理变得简单直接。

例如，要查找所有正在运行的服务：

```
PS > Get-Service | Where-Object { $_.Status -eq "Running" }

Status   Name               DisplayName
------   ----               -----------
Running  ADAM_Test          Test
Running  ALG                Application Layer Gateway Service
Running  AudioSrv           Windows Audio
Running  BITS               Background Intelligent Transfer Ser...
Running  Browser            Computer Browser
Running  COMSysApp          COM+ System Application
Running  CryptSvc           Cryptographic Services
```

或者，按依赖它们的服务数量对服务进行排序：



PS > Get-Service | Sort-Object -Descending { $_.DependentServices.Count }

```
Status   Name               DisplayName
------   ----               -----------
Running  RpcSs              Remote Procedure Call (RPC)
Running  PlugPlay           Plug and Play
Running  lanmanworkstation  Workstation
Running  SSDPSRV            SSDP Discovery Service
Running  TapiSrv            Telephony
(...)
```

由于 PowerShell 返回代表系统服务的高保真 .NET 对象，这些任务及更多任务变得异常简单，因为 PowerShell 为每个服务返回了丰富的信息。有关 `Get-Service` cmdlet 的详细信息，请输入 `Get-Help Get-Service`。有关 PowerShell 命令中的筛选、分组和排序的详细信息，请参阅 配方 2.2。

### 注意

`Get-Service` cmdlet 显示有关正在运行的服务的大部分（但并非全部）信息。若要获取其他信息（如服务的进程 ID），请使用 `Get-CimInstance` cmdlet：

```
$service = Get-CimInstance Win32_Service |
    Where-Object { $_.Name -eq "AudioSrv" }
$service.ProcessID
```

除了支持本地计算机上的服务外，`Get-Service` cmdlet 还允许您检索和管理远程计算机上的服务：

```
PS > Get-Service -Computer <Computer> |
    Sort-Object -Descending { $_.DependentServices.Count }

Status   Name               DisplayName
------   ----               -----------
Running  RpcEptMapper       RPC Endpoint Mapper
Running  DcomLaunch         DCOM Server Process Launcher
Running  RpcSs              Remote Procedure Call (RPC)
Running  PlugPlay           Plug and Play
Running  nsi                Network Store Interface Service
Running  SamSs              Security Accounts Manager
(...)
```

有关使用 .NET Framework 类的详细信息，请参阅 配方 3.8。有关使用 `Get-CimInstance` cmdlet 的详细信息，请参阅 第 28 章。

## 另请参阅

- 配方 2.2，“筛选列表或命令输出中的项目”
- 配方 3.8，“使用 .NET 对象”
- 第 28 章

# 25.2 管理正在运行的服务

## 问题

您想要管理正在运行的服务。

## 解决方案

要停止服务，请使用 `Stop-Service` cmdlet：

```
PS > Stop-Service AudioSrv -WhatIf
What if: Performing operation "Stop-Service" on Target "Windows Audio
(AudioSrv)".
```

同样，使用 `Suspend-Service`、`Restart-Service` 和 `Resume-Service` cmdlet 分别挂起、重启和恢复服务。

## 讨论

`Stop-Service` cmdlet 允许您通过名称或显示名称停止服务。

有关 `Stop-Service` cmdlet 的详细信息，请输入 `Get-Help Stop-Service`。如果要挂起、重启或恢复服务，请分别参阅 `Suspend-Service`、`Restart-Service` 和 `Resume-Service` cmdlet 的帮助。

### 注意

请注意，解决方案在 `Stop-Service` cmdlet 上使用了 `-WhatIf` 标志。此参数可让您查看运行命令时会发生什么，但不会实际执行该操作。

要配置服务（例如其描述或启动类型），请参阅 配方 25.3。除了允许您配置服务外，该配方中描述的 `Set-Service` cmdlet 还允许您在远程计算机上停止服务。

## 另请参阅

- 配方 25.3，“配置服务”
- 第 28 章

# 25.3 配置服务

## 问题

您想要配置服务的属性或启动行为。

## 解决方案

要配置服务，请使用 `Set-Service` cmdlet：

```
Set-Service WinRM -DisplayName 'Windows Remote Management (WS-Management)' `
    -StartupType Manual
```

要创建新服务或卸载现有服务，请使用 `New-Service` 和 `Remove-Service` cmdlet。

## 讨论

`Set-Service` cmdlet 允许您管理服务的配置：其名称、显示名称、描述和启动类型。

如果您更改了服务的启动类型，自然的下一步是验证更改是否已正确应用。配方 25.1 展示了如何查看服务的属性，包括启动类型。

## 另请参阅

- 配方 25.1，“列出所有正在运行的服务”
