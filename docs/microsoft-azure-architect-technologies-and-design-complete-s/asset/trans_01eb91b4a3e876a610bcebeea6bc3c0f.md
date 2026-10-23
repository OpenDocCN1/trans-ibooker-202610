

## 练习 3.7

### 从 Azure 虚拟机连接 Azure SQL

- 1. 通过远程桌面连接到已安装表 3.11 中所标识实用程序的 Azure 虚拟机。

- 2. 打开命令提示符并输入以下内容，也在图 3.34 中进行了可视化。`sqlcmd` 默认安装在 `C:\Program Files\Microsoft SQL Server\Client SDK\ODBC\170\Tools\Binn`。导航至该位置运行 `sqlcmd`，或将路径添加到环境变量中，如我所做，这允许从任何位置执行。请务必将 `<uid>` 和 `<password>` 替换为您的凭据。

  ```
  sqlcmd -S *.database.windows.net -d <dbname> -U <uid> -P <password>
  select database_id, is_read_only, compatibility_level from sys.databases
  go
  ```

**图 3.34** 从 Azure 虚拟机连接 Azure SQL

![](img/95043b3df926bcdadadb082b49262037_206_0.png)

- 3. 从 Azure 虚拟机成功执行 SELECT 查询证实连接成功。

- 4. 图 3.34 中的第二个命令提示符输出是从我的本地工作站运行的。请记住，在创建 Azure SQL 数据库时，您需要允许 Azure 服务访问数据库。由于我的工作站的 IP 地址不是 Azure 服务，因此连接请求被拒绝。要使其工作，客户端 IP 地址需要在数据库上进行白名单设置。您将在第 5 章中了解更多关于此的信息。

Azure SQL 数据库实例的终结点是公开的，但它们并非简单地对数据库连接开放；需要凭据。Azure App Service 的情况类似；然而，由于 App Service 实例是客户通常在上面运行互联网应用程序的位置，因此默认情况下没有实施 IP 限制。这是可能的；Azure App Service 具有添加白名单或黑名单 IP 地址列表的功能。请还记住，在 Azure 资源之间建立连接时，无论它们是否有全局终结点，流量都会在 Microsoft 全球网络内部路由。

尝试从同一 Azure 虚拟机和非 Azure 的工作站使用 curl 访问 Azure Web 应用，如下所示：

```
curl -G csharpguitar.azurewebsites.net/api/values
```

输出应类似于以下内容：

```
["csharp","guitar"]
```

在练习 3.8 中，让我们为 Azure Web 应用配置服务终结点，以便其终结点仅可从 VNet 内部访问。回想一下图 3.13 和练习 3.2，在您创建 VNet 时，您还从服务终结点下拉菜单中启用了 Microsoft.Web。这向当时绑定到 Azure 虚拟机的表添加了一条路由。您可能考虑在启用服务终结点之前运行 PowerShell cmdlet 并检查路由表，然后在启用后再次检查，只是为了养成习惯并确认配置按预期工作。

## 练习 3.8

### 为 Azure Web 应用配置服务终结点

- 1. 从 Azure 网络之外的任何工作站或客户端使用 curl，如图 3.35 所示。这是为了确保访问未受限制，以便您在启用服务终结点后将访问尝试进行比较。

  `curl -G csharpguitar-se.azurewebsites.net/api/values`

**图 3.35** 从 Azure 虚拟机连接到 Azure Web 应用

![](img/95043b3df926bcdadadb082b49262037_207_0.png)

- 2. 登录到 Azure 门户 `portal.azure.com`。

- 3. 按照以下说明创建 Azure App Service，并将 Azure Web 应用放置在与要配置服务终结点的 Azure 虚拟机和 VNet 相同的资源组中。

  `docs.microsoft.com/en-us/azure/app-service/`

- 4. 导航到为本练习创建的 Azure Web 应用。选择 **Networking** > **Access Restrictions**。配置 **Access Restrictions** 并单击 **+ Add rule**。

- 5. 添加名称（例如，`ALLOW CSHARPGUITAR VNET A GUITAR`）；然后选择 **Allow**，将优先级设置为 300，并提供描述。从 **Type** 下拉列表中选择 **Virtual Network**，选择订阅，选择虚拟网络（例如，`CSHARPGUITAR-VNET-A`），从 **Subnet** 下拉列表中选择 `guitar`，然后单击 **Add Rule** 按钮。注意显示 Microsoft.Web 服务终结点将在所选子网（guitar）上启用的消息。

- 6. 导航到子网详细信息，您将看到它确实已被选中。此外，检查有效路由，您将看到添加了 *VirtualNetworkServiceEndpoint* 下一跳类型。

- 7. 执行与步骤 1 相同的 curl；您现在应该会收到 403 – Forbidden HTTP 状态响应。使用 RDP 通过远程桌面连接到 `CSHARPGUITAR-VNET-A` 和子网 `guitar` 内的 Azure 虚拟机并执行相同的步骤，请求将成功。

运行在 csharpguitar、csharpguitar-se 和 Azure Web 应用上的源代码是 ASP.NET Core 2.2 Web API 的默认版本；说明可以在以下网站找到：

docs.microsoft.com/en-us/aspnet/core/tutorials/first-web-api

如有需要，可对默认的 `ValuesController.cs` 进行少量更改以自定义它。

第 8 章将更详细地解释如何将 API 部署到 Azure App Service 平台。服务终结点，如图 3.36 所示，是一种在子网上授予众多 Azure 产品入站网络流量的方式，同时拒绝从互联网访问这些终结点。

这对于保护具有全局终结点的 Azure 功能而言，是一个有用的额外安全层。我特别使用了入站来指出，仅因为 VNet 支持入站流量的连接，并不意味着使用私有 IP 地址的出站流量会自动配置。例如，使用 curl 测试的 Azure 虚拟机具有 10.0.1.4 的私有 IP 地址；出站流量是到现在受保护的全局终结点。然而，该 Azure Web 应用将无法使用 10.0.1.4 访问 Azure 虚拟机。如果 NSG 设置为允许，则 Azure 虚拟机的公共 IP 地址是可访问的。支持服务终结点的众多终结点各自设置了自己的配置要求和细微差异，但目的在每种情况下都是相同的，即防止全局终结点上的入站互联网流量。

另一个术语，子网委派，可以在配置服务终结点的同一刀片上找到。子网委派允许在子网中创建特定于服务的资源。例如，*Microsoft.Batch/batchAccounts* 资源。此资源具有读取、写入和删除等特权。当您将该资源委派给子网时，Azure Batch 产品将在子网内的资源上拥有这些权限。

**图 3.36** 服务终结点的可视化

![](img/95043b3df926bcdadadb082b49262037_209_0.png)

## VNet 到 Internet

出站互联网连接默认启用，您无需执行任何操作即可打开它。使用 NSG 可以限制某些端口和协议，该 NSG 已讨论过并将在下一节中详细阐述。测试连接最简单的方法是在 www.bing.com 或 www.stackoverflow.com 等网站上使用 curl。或者打开浏览器并导航到 URL。
回想一下 CIDR 前缀为 0.0.0.0/0 的路由表，其定义下一跳为互联网。当目标 IP 地址不属于其他任何路由时，就会匹配此路由。请求会从 Microsoft 全球网络路由到互联网骨干。现在您应该了解 Azure VNet 和子网。剩余部分涵盖安全、DNS、混合网络以及一些其他通用网络产品和功能。请继续阅读。

## 网络安全

关于 Azure 上可用的网络安全功能已经提出了一些观点。例如，我们谈到了网络安全组（NSG）、应用程序安全组（ASG）、网络过滤、网络虚拟设备（NVA）和服务终结点。在本节中，我们将更深入地了解与 Azure 相关的网络安全；具体如下：

- 使用 NSG、ASG 和 NVA 进行流量过滤
- Azure Application Gateway/WAF

