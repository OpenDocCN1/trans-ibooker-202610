

`Standard_LRS` 指的是本地冗余存储冗余选项。在此配置下，数据在单个 Azure 区域内复制三次，但不会跨区域复制。这可确保抵御硬件故障的耐久性，但不提供地理冗余。

如果您遇到 `MissingSubscriptionRegistration` 错误，请通过运行命令 `az provider register --namespace Microsoft.Web` 注册所需的资源提供程序。有关更多详细信息和故障排除指导，请参阅 [Microsoft 文档](https://oreil.ly/4BYHX)。

截至撰写本文时，Azure Functions v4 支持 Node.js 20 和 Node.js 22。您可以在 [“Azure Functions Runtime Versions Overview” 页面](https://oreil.ly/N5XVV) 上查看在创建函数应用时支持的语言和语言版本。

本章假设您使用的是 `func` 版本 4.0.X。更具体地说，本章中的动手示例是使用版本 4.0.7317 准备的。

在本章中，我们使用 Ruby 进行演示，但您可以使用任何您熟悉的编程语言，例如 Python。

如果您遇到 `MissingSubscriptionRegistration` 错误，请通过运行命令 `az provider register --namespace 'Microsoft.KeyVault'` 注册所需的资源提供程序。有关更多详细信息和故障排除指导，请参阅 [Microsoft 文档](https://oreil.ly/4BYHX)。

您可以在 [“Azure Role-Based Access Control (Azure RBAC) vs. Access Policies (Legacy)” 页面](https://oreil.ly/8QADb) 上查看更多信息。

*principal ID* 在 Azure 中唯一标识一个安全主体，例如托管标识，用于在分配和管理资源权限时使用。

有关 Azure Key Vault RBAC 和内置角色的更多详细信息，请参阅 [Azure RBAC guide](https://oreil.ly/PFrQy)。

本章假设您使用的是 `@azure/identity` `^4.12.0` 和 `@azure/keyvault-secrets` `^4.10.0`。

这反映了 Azure 中管理平面和数据平面之间的差异。*管理平面* 处理创建或配置资源等操作。*数据平面* 处理对资源实际内容的实际操作，例如在 Key Vault 中读取或写入密钥。访问令牌的范围取决于您想要交互的平面而有所不同。

# 第 12 章. 在 Microsoft Azure 中提升权限

在上一章的模拟中，您利用了无服务器函数中的代码注入漏洞，获取了与该函数托管标识关联的访问令牌，该标识具有 Owner 级别权限。实际上，大多数与无服务器函数和其他支持无服务器操作模型的云资源关联的托管标识被授予的访问权限要有限得多。

在本章中，您将在所学知识的基础上，探讨攻击者如何通过滥用过于宽松的托管标识在 Azure 中提升权限。您将首先升级之前设置的有意易受攻击的无服务器函数，并将其托管标识配置为更严格的角色集。接下来，您将执行模拟攻击并通过代码注入提取函数应用的托管标识令牌，通过管理平面分配提升的角色来提升权限，然后从 Key Vault 访问密钥。最后，您将通过删除任何不必要的角色分配来保护函数，以强制执行最小权限原则。

本章将涵盖以下内容：

*   [“Updating the Function App Permissions”](#ch12.html#updating_the_function_app_permissions_ch12-escalating-privileges-in-microsoft-azure_1764871097889)
*   [“Escalating Privileges Through an Overly Permissive Managed Identity”](#ch12.html#escalating_privileges_through_an_overly_permissive_managed_identity_ch12-escalating-privileges-in-microsoft-azure_1764871116519)
*   [“Applying Least Privilege to the Function App Managed Identity”](#ch12.html#applying_least_privilege_to_the_function_app_managed_identity_ch12-escalating-privileges-in-microsoft-azure_1764871129343)

到本章结束时，您应该能够识别可能允许攻击者在 Azure 中提升权限的配置错误。这将帮助您在发生之前防止权限提升并加强 Azure 无服务器应用程序的整体安全态势。

## Reviewing Technical Prerequisites

要跟随本章，您需要以下内容：

*   在本地计算机上安装的代码编辑器（例如 VS Code 或 Sublime Text）
*   两个 Microsoft Azure 账户：一个用于设置和托管易受攻击的无服务器实验室环境（Serverless Lab 账户），另一个用于运行模拟利用（Attacker 账户）

在跟随本章时，请确保两个账户都已准备就绪并在单独的浏览器会话（或隐身/隐私窗口）中打开，以避免会话冲突。

###### Note

为了帮助您完成练习和模拟，本章中使用的代码和命令的副本可在 [GitHub gist（见 `ch12.md`）](https://oreil.ly/0mSPU) 中获得。

## Updating the Function App Permissions

在本节中，您将更新函数应用托管标识的角色分配，用更不宽松的一组角色 Contributor 和 User Access Administrator 替换其当前分配的角色 Owner 和 Key Vault Secrets Officer。请记住，虽然这些角色权限更低，但攻击者如果利用无服务器函数中的代码注入漏洞，可能会利用托管标识的权限来提升权限（[Figure 12-1](#ch12.html#fig1201)）。

由于函数应用的托管标识不再具有对 Key Vault 的访问权限，您还将相应地更新 `evaluate` 函数代码，然后重新部署该函数。

![](assets/lsse_1201.png)

在动手操作之前，让我们讨论本章的几个关键术语：

Managed identity

: 

分配给 Azure 资源以便在不将凭据存储在代码中的情况下对其他 Azure 服务进行身份验证的标识。这类似于给员工一张公司 ID 卡，该卡可自动授予其访问批准的房间和资源的权限，而无需为每扇门准备单独的钥匙。在此类比中，托管标识是公司 ID 卡，而 Azure 资源，例如无服务器函数，则是使用它访问其他服务和资源的员工。

Subscription

: 

在该 Azure 账户下创建和计费资源的 Azure 账户。继续这个类比，订阅就像管理 ID 卡（托管标识）的公司，并决定哪些员工（Azure 资源）可以访问哪些房间和资源。

Subscription ID

: 

Azure 订阅的唯一标识符。订阅 ID 就像公司的注册号或税号，唯一标识它与其他公司不同。

Principal

: 

可以分配角色以访问资源的安全实体，例如用户、组或托管标识。您可以将主体视为可以被授予访问公司资源和进入建筑物特定房间权限的任何人（或任何事物）。

Principal ID

: 

主体的唯一标识符。您可以将其视为唯一的员工 ID 或徽章号码，用于区分一个人或服务，即使多个主体具有相似的名称或角色。

Role assignment
:

