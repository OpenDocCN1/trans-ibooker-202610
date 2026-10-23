

![](img/4fa338e84e0a590967a142dd798ba4e2_287_0.png)

图7.14 – AZ CLI登录结果

6. 运行以下命令以获取租户级访问权限。当提示时，使用之前使用的同一全局管理员用户进行身份验证：

```
az login --allow-no-subscriptions
```

这次你会收到一条关于成功登录的消息。以下是输出截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_287_1.png)

图7.15 – 作为全局管理员成功登录

7. 运行以下命令，利用全局管理员角色修改对Azure资源的权限：

```
az rest --method post --url "/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01"
```

如果没有收到任何输出，则表示命令已成功执行。

8. 注销，然后使用全局管理员凭据重新登录，以确认你现在对Azure订阅资源拥有权限：

```
az logout
```

```
az login
```

以下是相关截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_288_0.png)

图7.16 – 具有订阅访问权限的AZ CLI登录

9. 使用以下命令将订阅所有者角色分配给全局管理员账户：

```
userPrincipalName=$(az ad signed-in-user show --query userPrincipalName -o tsv)
```

```
az role assignment create --role "Owner" --assignee $userPrincipalName
```

命令将成功执行，证明你已经使用提升后的全局管理员角色修改了Azure资源的权限！以下是命令的输出：

```
root@PentestVM:~#
root@PentestVM:~# userPrincipalName=$(az ad signed-in-user show --query userPrincipalName -o tsv)
root@PentestVM:~# az role assignment create --role "Owner" --assignee $userPrincipalName
{
  "canDelegate": null,
  "condition": null,
  "conditionVersion": null,
  "description": null,
  "id": "/subscriptions/204cce89-.../providers/Microsoft.Authorization/roleAssignments/5ac7db39-887b-40c6-a743-e562f2120a6a",
  "name": "5ac7db39-887b-40c6-a743-e562f2120a6a",
```

图7.17 – 分配所有者角色

现在，我们已经从Azure AD修改了自身对Azure订阅的权限。让我们探讨相反的场景以及可用于实现这一目标的技术。

## 从订阅所有者提升到Azure AD角色

在渗透测试中，你可能还希望从Azure订阅横向移动到Azure AD。这一目标可能是攻击链的一部分，其目的是在Azure AD中打开后门以实现持久化。我们将在第8章“在Azure环境中实现持久化”中讨论持久化主题。以下是可用于实现此目标的一些技术。

## 路径1 – 利用特权服务主体

与用户账户类似，服务主体和托管标识也可以被分配到Azure AD角色。许多攻击者认为服务主体和托管标识是更容易的目标，因为它们通常被排除在条件访问和MFA等安全策略之外。

攻击者可以利用具有服务主体或托管标识权限的Azure AD账户的特权，访问安全特权主体。这是一条可能路径，但你可能很少遇到，尤其是当组织严格遵守不同作用域分离角色分配的原则时。

## 路径2 – 利用服务主体的API权限

Microsoft标识平台支持两种类型的服务主体权限：委托权限和应用权限。委托权限用于交互式身份验证流程，其中存在已登录用户。在这种情况下，用户需要在访问时同意权限（或者管理员可以代表用户同意）。

应用权限用于在没有任何用户交互的情况下运行的应用程序，这意味着没有用户登录到这些应用。管理员只需为应用同意一次授予的权限。服务主体被授予对API（如Microsoft Graph）执行敏感操作的权限并不罕见。攻击者可以利用这些权限访问应用已被授予访问权限的API。

![](img/4fa338e84e0a590967a142dd798ba4e2_290_0.png)

图7.18 – 服务主体应用权限

例如，一个应用可能在Azure AD中没有任何角色分配，但如果它具有在Microsoft Graph中执行敏感操作的应用权限，这就可以作为访问Azure AD的间接方式。这种攻击方式也正成为攻击者常用的网络钓鱼技术。通过使用Azure AD同意授权URL（图7.19），攻击者可以使用合法的Azure URL发送给网络钓鱼受害者。此URL使用Azure AD内置的权限批准流程，允许用户批准外部应用程序。

## 请求的权限

OAuth令牌攻击

此应用程序不是由Microsoft发布的。

此应用希望：

- 让你登录并读取你的个人资料
- 保留你对已授权数据的访问权限
- 读取你的邮件

接受这些权限意味着你允许此应用按照其服务条款和隐私声明使用你的数据。你可以在 https://myapps.microsoft.com 更改这些权限。显示详细信息

此应用看起来可疑吗？请在此处举报

图7.19 – OAuth网络钓鱼攻击示例

从与所有者相关的Azure AD提升场景继续，我们现在将重点介绍利用本地网络的提升技术。

## 攻击本地系统以在Azure中提升权限

虽然这似乎偏离了攻击Azure服务，但许多租户与其同步的Active Directory环境存在直接连接。通过利用这些连接攻击本地系统，攻击者可能能够提升其在主Active Directory环境中的权限。提升到域管理员权限将使攻击者能够访问域中特权用户的账户，包括那些具有Azure AD权限的用户。

由于在内部环境中提升权限的方法有很多，我们将重点介绍一些通用概念，这些概念将用于从Azure横向移动到本地系统。

## 识别与本地网络的连接

首先，我们将了解如何识别连接云与本地网络的连接。这些连接可以让我们横向移动到其他网络，从而可能在一般Active Directory环境中进行权限提升。

> **重要说明**
> 
> 由于此场景的实验室需要你设置到另一个网络的连接，我们不会在动手练习中介绍此内容。但我们会提供如何查找这些连接的说明。

在Azure门户中，查找本地连接是一项简单的任务。只需导航到“连接”边栏选项卡，即可看到可用连接列表。这些连接可以是“站点到站点VPN”或“Express Route”连接。

## 站点到站点（IPsec）连接

站点到站点连接依靠传统网络硬件（VPN集中器、路由器等）创建从Azure到另一个网络的VPN隧道。许多组织选择此选项，因为它允许他们与现有技术集成。站点到站点的主要区别之一是流量通过公共互联网传输，而不是私有骨干连接。

## Express Route连接

对于那些希望从其网络到Azure拥有更专用（且私有）连接的用户，Express Route连接是首选选项。这些连接通常通过Microsoft有合作关系的服务提供商为组织创建。

虽然了解这些连接的工作原理很重要，但我们真正关心的是如何利用这些连接攻击其他网络。由于创建这些连接需要使用虚拟网关，我们需要检查网关设置，以确定哪些网络可以访问远程连接。

