

以下是输出的截图和要删除的行：

![](img/4fa338e84e0a590967a142dd798ba4e2_251_0.png)

图 6.40 – 删除 .bashrc 文件的修改

现在我们已经看到了 Cloud Shell 如何被滥用，让我们来看一下如何从 Azure Key Vaults 中收集敏感信息。

## 从 Key Vaults 中掠夺密钥、机密和证书

Azure Key Vault 是在 Azure 平台中管理应用程序机密、密钥和证书的服务（图 6.41）。Key Vault 的主要用例是为应用程序开发人员提供一种安全的方式，将其代码在运行时需要使用的敏感信息集中存储，而无需将该信息作为代码的一部分。常见存储在 Key Vault 中的敏感信息可能是令牌、密码、数据存储连接字符串、API 密钥和其他机密类型。

![](img/4fa338e84e0a590967a142dd798ba4e2_251_1.png)

图 6.41 – 应用程序如何访问 Key Vault

正如你所能想象的，像 Key Vault 这样的服务通常是攻击者的热门目标，因为它可能包含可用于在攻击链中横向移动或用于解密加密信息的信息。

对 Key Vault 资源的访问通过两个平面进行控制（图 6.42） – 管理平面，其端点为 `management.azure.com`，以及数据平面，其端点为 `<vault-name>.vault.azure.com`（图 6.42）。**管理平面**是管理员与之交互以执行管理操作的端点，例如创建和删除 Key Vault 实例、检索 Key Vault 属性以及更新访问策略。

**数据平面**是应用程序与之交互以访问存储在保管库中的敏感信息（机密、密钥和证书）的接口。在此平面上可以执行的操作包括读取、添加、删除或修改密钥、机密和证书。

![](img/4fa338e84e0a590967a142dd798ba4e2_252_0.png)

*图 6.42 – 管理平面与数据平面访问*

管理平面和数据平面都使用 **Azure Active Directory（Azure AD）** 进行身份验证，但其授权方式不同。管理平面使用 **Azure 基于角色的访问控制（Azure RBAC）** 管理权限，而数据平面使用 Key Vault 访问策略，但也可以使用 Azure RBAC 数据权限（见以下说明）。微软故意采用这种方法，以便组织能够将需要管理级权限的角色与需要访问敏感信息的角色分开。这里的思路是，对管理平面的访问不一定会给予对数据平面的访问。

> **重要说明**
> 在本书撰写之时，有一个新发布的特性允许使用 Azure RBAC 来控制对数据平面的授权访问。微软新增了九个新的服务级别角色，包括 Key Vault Administrator、Key Vault Certificates Officer 和 Key Vault Crypto Officer，这些角色授予在数据平面上执行不同级别操作的权限。在你的侦察活动中请记住这些角色。

从攻击角度来看，Contributor 角色默认对 Key Vault 资源的 data plane 没有任何权限。然而，我们可以利用其更新访问策略的权限来提升对数据平面的权限 – `Microsoft.KeyVault/vaults/accessPolicies/write`。诸如 MicroBurst、PowerZure 和 Lava 之类的工具利用此功能来转储 Key Vault 资源中的敏感信息。

在审查密钥保管库时，首先要查看访问策略，看看你的当前用户账户是否对保管库有权限。如果你的用户没有保管库的权限，并且你试图保持隐蔽，你可能需要查看如何使用其他授权的安全主体访问保管库。在许多情况下，这些权限会应用于自动化 `Run as` 账户（见本章后面）、应用注册和/或托管标识。

以下是使用替代主体访问密钥保管库的一些选项：

- **自动化账户**：创建一个使用 `Run as` 账户访问密钥保管库的新运行手册。
- **应用注册**：以应用注册身份进行身份验证并访问密钥保管库。
- **托管标识**：为该标识生成 REST API 令牌以访问密钥保管库。

这些选项中的每一个允许你访问保管库的方式都非常不同，但与更改访问策略相比，它们可能是访问保管库更隐蔽的选择。

在下一个动手练习中，我们将使用 MicroBurst 利用 Contributor 角色大规模利用 Key Vault 资源。

5. 提示时，选择 Azure 订阅并单击确定：

![](img/4fa338e84e0a590967a142dd798ba4e2_255_0.png)

图 6.44 – 选择订阅

6. MicroBurst 然后将枚举所有 Key Vault 资源，添加访问策略以临时授予权限，窃取其中的信息，然后删除添加的访问策略。

```
PS C:\Users\pentestadmin\MicroBurst> Get-AzPasswords -StorageAccounts N -AutomationAccounts N -AppServices N -ACR N -CosmosDB N -ModifyPolicies Y -Verbose | Out-GridView
VERBOSE: Logged In as contributor@azurepentesting.com
VERBOSE: Getting List of Key Vaults...
VERBOSE: Starting on the azurepentesting Key Vault
VERBOSE: Current user does not have an access policy entry in the azurepentesting vault, adding get/list rights
VERBOSE: Exporting items from azurepentesting
VERBOSE: Removing current user from the Access Policies for the azurepentesting vault
VERBOSE: Starting on the azptkv2881 Key Vault
VERBOSE: Current user does not have an access policy entry in the azptkv2881 vault, adding get/list rights
VERBOSE: Exporting items from azptkv2881
VERBOSE: Getting Key value for the disk-encryption-key Key
VERBOSE: Getting Secret value for the db-encryption-key Secret
VERBOSE: Getting Secret value for the SQLAdminPassword Secret
VERBOSE: Getting Secret value for the twitter-api-key Secret
VERBOSE: Removing current user from the Access Policies for the azptkv2881 vault
VERBOSE: Password Dumping Activities Have Completed
```

图 6.45 – 示例 Get-AzPasswords 输出

以下是窃取信息的截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_256_0.png)

图 6.46 – Get-AzPasswords 输出在 gridview 中显示

值得注意的是，一些机密的值中会有其所属资源的信息。例如，数据库或存储账户连接字符串。而其他机密将只是机密信息本身，没有其所属资源或服务的上下文。

一种了解机密被使用位置的方法是审查访问策略。这将允许我们看到哪些应用程序/资源正在使用这些敏感信息。例如，如果一个名为 app1 的托管标识具有读取保管库中机密的访问策略分配，你可能会推断出分配了 app1 托管标识的资源会使用这些机密。你也可以审查资源的诊断日志（如果已启用并且你的凭据有权读取）以查看 API 调用来自何处。

使用 Azure Key Vault 和 Contributor 角色的另一种攻击模式是添加一个访问策略，允许我们在供应链攻击中生成受信任的证书。密钥保管库允许客户与受支持的第三方证书颁发机构集成，例如 Digicert 和 Globalsign，以简化组织受信任 TLS 证书的签发和管理（图 6.47）：

![](img/4fa338e84e0a590967a142dd798ba4e2_256_1.png)

图 6.47 – Key Vault 证书生成流程

请将本章加入书签，因为密钥保管库通常包含在 Azure 环境中提升权限所需的机密。除密钥保管库外，还有 Azure Web 应用，它们经常包含可用于权限提升和横向移动的凭据（和托管标识）。

## 利用 Web 应用进行横向移动和提升权限

Azure Web 应用通常在订阅中用于托管 Web 应用程序和 API。虽然我们之前提到过滥用托管标识的用法，但我们尚未介绍应用程序通常如何被管理。

应用程序代码可以通过多种不同的方式应用到 App Service 主机。部署中心可以与多种不同的代码存储库解决方案集成以与 CI/CD 管道同步，或代码可以通过手动部署推送。

![](img/4fa338e84e0a590967a142dd798ba4e2_257_0.png)

图 6.48 – App Service 部署中心

