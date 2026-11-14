
## 事务支持

在注册表中工作时，有时可能希望将一组相关更改串联起来，并确保它们作为一个整体单元应用。这些目标称为*原子性*和*一致性*：即希望避免在操作的任何步骤中发生错误，如果其他操作也不成功，可能导致系统状态不一致。

为了支持此类管理任务，PowerShell 支持一种称为*事务*的更改管理策略。PowerShell 的注册表提供程序完全支持事务。

启动事务时，该事务中的任何命令都是虚拟的，直到完成事务才会真正应用到系统。在事务上下文中，每个参与的命令看到的系统状态就好像真的已经更改了一样。完成事务后，更改将作为一个整体单元应用。

某些支持事务的系统（如数据库）会对事务正在更改的任何资源加锁。如果另一个用户尝试修改被锁定的资源，会收到错误消息。Windows 注册表不支持此功能。如果某事物更改了事务所依赖的资源，事务中包含的更改将被放弃，尝试完成该事务时会收到错误消息。有关事务的详细信息，请参阅[第 30 章](#ch30.xhtml#chapter_transactions)。

## 另请参阅

[第 30 章](#ch30.xhtml#chapter_transactions)

# 21.7 将站点添加到 Internet Explorer 安全区域

## 问题

你想将站点添加到特定的 Internet Explorer 安全区域。

## 解决方案

要创建将站点添加到特定安全区域所需的注册表项和属性，请使用 `New-Item` 和 `New-ItemProperty` cmdlet。[示例 21-3](#ch21.xhtml#adding_httpwwwexamplecom_to_the_list_of_) 将 *www.example.com* 添加到 Internet Explorer 信任的站点列表中。

### 示例 21-3. 将 www.example.com 添加到 Internet Explorer 的受信任站点列表

```
Set-Location "HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
Set-Location ZoneMap\Domains
New-Item example.com
Set-Location example.com
New-Item www
Set-Location www
New-ItemProperty . -Name http -Value 2 -Type DWORD
```

## 讨论

需要修改注册表中的数据的任务之一是使用 Internet Explorer 从其不同的安全区域添加和删除站点。

Internet Explorer 将其区域映射信息存储在注册表的 `HKCU:\Software\Microsoft\Windows\CurrentVersion\InternetSettings\ZoneMap\Domains` 中。在该键下，资源管理器存储域名（如 `leeholmes.com`），主机名（如 `www`）作为其子键（参见[图 21-1](#ch21.xhtml#internet_explorer_zone_configuration)）。在主机键中，资源管理器存储一个属性（如 `http`），其 `DWORD` 值对应于区域标识符。

Internet Explorer 区域标识符如下：

- 我的电脑
- 本地 Intranet
- 受信任的站点
- Internet
- 受限制的站点

![](Images/wps4_2101.png)

当 Internet Explorer 配置为增强安全配置模式时，还必须更新 `EscDomains` 键下的条目。

> **注意**
>
> 一旦计算机启用了 Internet Explorer 的增强安全配置，即使移除增强安全配置，这些设置仍会保留。以下命令可让计算机再次信任 UNC 路径：
>
> ```
> Set-Location "HKCU:\Software\Microsoft\Windows\"
> Set-Location "CurrentVersion"
> Set-Location "Internet Settings"
> Set-ItemProperty ZoneMap UNCAsIntranet -Type DWORD 1
> Set-ItemProperty ZoneMap IntranetName -Type DWORD 1
> ```

要删除特定域的区域映射，请使用 `Remove-Item` cmdlet：

```
PS > Get-ChildItem

   Hive: HKEY_CURRENT_USER\Software\...\Internet Settings\ZoneMap\Domains

SKC VC Name                            Property
--- -- ----                            --------
  1  0 example.com                     {}

PS > Remove-Item -Recurse example.com
PS > Get-ChildItem
PS >
```

有关使用 Internet Explorer 注册表条目配置安全区域的详细信息，请参阅 Microsoft KB 文章[“Internet Explorer Security Zones Registry Entries for Advanced Users”](http://support.microsoft.com/kb/182569)。有关管理 Internet Explorer 增强安全配置的详细信息，请在[Microsoft 官方文档站点](http://technet.microsoft.com)上搜索。

有关修改注册表中数据的详细信息，请参阅[配方 21.3](#ch21.xhtml#modify_or_remove_a_registry_key_value)。

## 另请参阅

[配方 21.3“修改或删除注册表键值”](#ch21.xhtml#modify_or_remove_a_registry_key_value)

# 21.8 修改 Internet Explorer 设置

## 问题

你想修改 Internet Explorer 的配置选项。

## 解决方案

要修改 Internet Explorer 配置注册表键，请使用 `Set-ItemProperty` cmdlet。例如，要更新代理：

```
Set-Location "HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
Set-ItemProperty . -Name ProxyServer -Value http://proxy.example.com
Set-ItemProperty . -Name ProxyEnable -Value 1
```

## 讨论

Internet Explorer 将其主要配置信息存储为注册表键 `HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings` 上的属性。要更改这些属性，请使用 `Set-ItemProperty` cmdlet，如解决方案中所示。

另一组常见的要调整的属性是定义安全区域的配置参数。一个例子是在受限站点区域中阻止脚本运行。对于每个区域，Internet Explorer 将此信息存储为注册表键 `HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Zones\<Zone>` 的属性，其中 `<Zone>` 表示要管理的区域标识符（0、1、2、3 或 4）。

Internet Explorer 区域标识符如下：

- 我的电脑
- 本地 Intranet
- 受信任的站点
- Internet
- 受限制的站点

此键中的属性名称并非为人类设计，因为它们带有诸如 `1A04` 和 `1809` 这样的启发性标题。虽然命名不佳，但仍可对其进行脚本编写。

有关使用 Internet Explorer 注册表设置配置安全区域的详细信息，请参阅 Microsoft KB 文章[“Internet Explorer Security Zones Registry Entries for Advanced Users”](http://support.microsoft.com/kb/182569)。

有关修改注册表中数据的详细信息，请参阅[配方 21.3](#ch21.xhtml#modify_or_remove_a_registry_key_value)。

## 另请参阅

[配方 21.3“修改或删除注册表键值”](#ch21.xhtml#modify_or_remove_a_registry_key_value)

# 21.9 程序：搜索 Windows 注册表

虽然 Windows 注册表编辑器可用于搜索注册表，但有时它可能无法提供所需的功能。例如，注册表编辑器不支持使用通配符或正则表达式进行搜索。

在文件系统中，我们有 `Select-String` cmdlet 来搜索文件内容。PowerShell 不为其他存储提供此功能，但我们可以编写脚本来实现。关键在于将注册表键值视为文件中的内容：

- 目录有项；项有内容。
- 注册表键有属性；属性有值。

[示例 21-4](#ch21.xhtml#search-registryps1) 遍历所有注册表键（及其值）以查找搜索词，并返回匹配信息。

### 示例 21-4. Search-Registry.ps1
