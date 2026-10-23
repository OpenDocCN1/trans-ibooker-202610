
它为我们提供了以下信息：

+   1. **User3** 最初对 Azure 资源没有访问权限，但拥有更新服务主体凭据的权限。
+   2. **App-1** 的服务主体被分配了 Azure 资源的参与者角色。
+   3. **User3** 获取 **App-1** 的应用 ID，并利用该权限向 **App-1** 添加新的客户端机密。
+   4. **User3** 使用应用 ID 和新添加的密钥获得对 Azure 资源的访问权限。

> **重要说明**
> 本章的讨论集中在 Azure AD 中的“全局管理员”角色上，但 Azure AD 中还有其他一些内置角色可用于执行此攻击，包括：**应用程序管理员**、**云应用程序管理员**、**目录同步账户**和**混合身份管理员**。此外，任何具有以下权限的 Azure AD 自定义角色都可能被利用：`microsoft.directory/servicePrincipals/credentials/update`。

此路径比重置用户密码的路径更受青睐，因为它侵入性较小。它不会影响使用服务主体的应用程序的任何现有访问权限，只是为攻击者增加了一种额外的访问方式。要识别具有 Azure 资源访问权限的服务主体，我们可以查看登录日志中的非交互式登录活动。此外，服务主体通常以其所使用的应用程序和/或订阅命名，因此其名称可能是一个很好的线索。

## 路径 4 – 提升对根管理组的访问权限

从 Azure AD 横向移动到 Azure 资源的另一条路径是提升对根管理组的访问权限。此选项是为了让全局管理员在所有者失去访问权限时重新获得对订阅的访问权限而存在的，但它也可能被攻击者利用。

提升操作隐式授予 **用户访问管理员** 角色对租户中所有订阅和管理组的访问权限，从而拥有向 Azure 资源分配角色的不受限制的权限（*图 7.5*）。利用此漏洞的攻击者可以查看所有资源，并在租户中的任何订阅或管理组中分配访问权限！

```
{
  "properties": {
    "roleName": "",
    "description": "",
    "assignableScopes": [
      "/subscriptions/204cce89-27de-4669-a48b-04c27255e05e"
    ],
    "permissions": [
      {
        "actions": [
          "*/read",
          "Microsoft.Authorization/*",
          "Microsoft.Support/*"
        ],
        "notActions": [],
        "dataActions": [],
        "notDataActions": []
      }
    ]
  }
}
```

用户访问管理员角色对 Microsoft.Authorization 具有完全访问权限。

图 7.5 – 用户访问管理员角色定义

可以通过 Azure 门户中 Azure AD 的属性部分完成提升（图 7.6）：

![](img/4fa338e84e0a590967a142dd798ba4e2_281_0.png)

图 7.6 – 修改对租户中所有订阅的访问权限

也可以通过 Azure CLI 以编程方式完成提升。相关命令如下：

```
az rest --method post --url "/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01"
```

> **重要说明**
> 微软提供了关于此功能在 Azure AD 中如何工作的出色文档。请参阅以下 URL 以获取更多信息：https://docs.microsoft.com/en-us/azure/role-based-access-control/elevate-access-global-admin#how-does-elevated-access-work。

现在我们已经了解了如何提升权限，接下来看一个实际示例。

## 动手实验 – 准备全局管理员/所有者利用场景

本动手实验将为我们完成本章其余实验做好准备。要跟随本章将介绍的场景进行操作，你需要先在 Azure AD 租户中设置一个具有全局管理员权限的用户，再在 Azure 订阅中设置一个具有所有者权限角色的用户。我们使用一个可以通过 Azure Cloud Shell 运行的 PowerShell 脚本自动完成了此设置。以下是本实验要完成的任务：

+   1. 打开 Web 浏览器并访问 Azure 门户（https://portal.azure.com）。使用 azureadmin 凭据登录。
+   2. 在 Azure 门户中，点击右上角的 Cloud Shell 图标。选择 PowerShell：
+   3. 在 Cloud Shell 窗格中的 PowerShell 会话中，运行以下命令，以下载一个脚本，用于创建具有所有者权限的用户账户、一个具有全局管理员权限的用户，并设置所需的易受攻击工作负载：

![](img/4fa338e84e0a590967a142dd798ba4e2_282_0.png)

图 7.7 – 点击图标以打开 Cloud Shell

```
PS C:\> Invoke-WebRequest https://bit.ly/owner-scenarios -O owner-scenarios.ps1
PS C:\> Get-ChildItem
```

以下是操作截图的示例：

![](img/4fa338e84e0a590967a142dd798ba4e2_283_0.png)

图 7.8 – 下载 Owner 场景脚本

4. 运行下载的脚本，以预配本章练习所需的对象和资源，使用以下命令：

```
PS C:\> ./owner-scenarios.ps1
```

当提示输入密码时，输入 myPassword123 并按 Enter。等待脚本部署完成。这可能需要大约 8 分钟：

![](img/4fa338e84e0a590967a142dd798ba4e2_283_1.png)

图 7.9 – 运行 Owner 场景脚本

## 这个脚本做什么？

该脚本会在 Azure AD 中创建一个具有全局管理员权限的用户账户，并在 Azure 订阅中创建另一个具有所有者权限的用户。请参见图 7.10 的示意图：

![](img/4fa338e84e0a590967a142dd798ba4e2_284_0.png)

图 7.10 – 全局管理员/所有者利用场景

5. 脚本完成后，本练习其余部分所需的关键信息将显示在输出部分。请将这些信息复制到记事本文档中，以备后用。你需要从输出部分获取四个值：

Azure Global Admin User：这是具有 Azure AD 全局管理员权限的管理员用户名。

**Azure Global Admin User Password：** 这是全局管理员用户的密码。

**Azure Owner User：** 这是对 Azure 资源具有所有者权限的管理员用户名。

**Azure Owner User Password：** 这是 Azure 资源所有者用户的密码。

相关信息可以在以下截图中看到：

![](img/4fa338e84e0a590967a142dd798ba4e2_285_0.png)

图 7.11 – 获取脚本输出

现在，你已经成功创建了将在本章后续练习中使用的资源。在下一个动手实验中，我们将介绍如何利用提升访问权限选项，从 Azure AD 横向移动到 Azure 资源。

## 动手实验 – 提升访问权限

以下是本实验要完成的任务：

+   - **任务 1：** 验证当前用户的访问权限。
+   - **任务 2：** 使用 Azure CLI 提升访问权限。
+   - **任务 3：** 验证对 Azure 资源的访问权限。

让我们逐步完成这些任务：

+   1. 在你的 Pentest VM 上打开 **PowerShell**。
+   2. 在 **PowerShell** 控制台中，输入 `bash` 切换到 **Windows Subsystem for Linux**。
+   3. 使用以下命令切换到 root 用户。当提示时，输入管理员密码：

```
azureuser@PentestVM:~# sudo su -
```

4. 使用 Azure CLI 命令 – az login 向 Azure 进行身份验证：

```
root@PentestVM:~# az login
```

以下是一个截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_286_0.png)

图 7.12 – az login 的结果

5. 当提示时，输入你在本章第一个练习中从脚本输出获取的 Azure 全局管理员凭据：

![](img/4fa338e84e0a590967a142dd798ba4e2_286_1.png)

图 7.13 – Azure 密码提示

你将收到一条关于无法访问订阅的输出消息。这是因为该账户仅有权访问 Azure AD，而无法访问任何 Azure 订阅：

