

21. 使用以下命令执行脚本：

```
./cleanup-test-users.ps1
```

输出如下所示：

![](img/4fa338e84e0a590967a142dd798ba4e2_130_0.png)

图 3.39 – 执行清理脚本

现在我们了解了如何猜测用户的凭据，让我们来看一下可能限制我们在 Azure 环境中使用这些凭据的额外情况。

## 条件访问策略

Azure AD 安全性的进步引入了条件访问策略的概念。这些策略可以根据管理员设定的某些条件限制对 Azure 门户的访问。方便的是，攻击者已经想出了绕过这些策略的方法，从而让我们能够访问 Azure 和 Microsoft 365 服务。

## 介绍 MFASweep

MFASweep 是一个方便的工具，我们可以用它来识别绕过条件访问策略的机会。

MFASweep 信息
创建者：Beau Bullock（Twitter：@dafthack）
开源或商业：开源项目
GitHub 仓库：https://github.com/dafthack/MFASweep
语言：PowerShell

虽然该工具在一定程度上超出了 Azure 的范围，但它可以作为外部渗透测试的一部分使用，以帮助利用可能受条件访问策略限制的凭据获得立足点。

该工具将遍历以下 Microsoft 服务，以识别任何允许在无需 MFA 或条件访问要求的情况下访问的服务：

- Microsoft Graph API
- Azure Service Management API
- Microsoft 365 Exchange Web Services
- Microsoft 365 Web 门户
- Microsoft 365 Web 门户，带移动用户代理（Android）
- Microsoft 365 ActiveSync

在一个真实场景中，我们可能会发现使用移动操作系统的用户对标准条件访问规则有例外。为什么会这样？一些移动用户会从不同的 IP 地址频繁登录，很难预测这些 IP。通过使用移动设备用户代理，我们可以假装成移动操作系统，从而绕过条件访问要求。

虽然我们不会在实验室中使用它，但将 MFASweep 工具用于测试是有益的：

1. 在你的渗透测试 VM 中，打开 PowerShell 控制台，切换目录到你的用户目录，并使用以下命令克隆 MFASweep GitHub 仓库：

```
cd C:\Users\$env:USERNAME\
git clone https://github.com/dafthack/MFASweep.git
```

2. 使用以下命令导入 MFASweep PowerShell 模块：

```
cd .\MFASweep\
Import-Module .\MFASweep.ps1
```

3. 使用以下命令对你已攻破的任何用户账户运行 MFASweep。对于允许绕过条件访问策略的账户，你应该会看到成功消息：

```
Invoke-MFASweep -Username sandra@azurepentesting.com -Password myPassword123
```

虽然条件访问策略绕过可能并非无处不在，但还有额外的 *非传统* 选项可用于绕过 MFA 要求。

## 额外的 MFA 绕过

每个测试人员凭据猜测的噩梦就是猜到一个需要 MFA 令牌的凭据。虽然这看起来像是巨大的挫折，但有一些方法可以绕过 MFA。

社会工程学是绕过 MFA 要求的一种选择。社会工程学是通过与目标进行沟通（通过电话、电子邮件、面对面交谈等）来获取信息或资源访问权限的行为。

通过发送精心设计的钓鱼邮件或电话，我们可能能够让用户在令牌仍然有效时与我们分享其 MFA 代码。或者，我们可以联系 IT 部门，假装成受害者账户，友好的 IT 人员可能会好心地为我们签发新的 MFA 令牌。如果你对社会工程学概念不太熟悉，我们已在 *Further reading* 部分末尾添加了更多信息的链接。

我们并不鼓励在所有渗透测试中都使用此方法，但通过反复使用猜测的凭据进行身份验证，你可能能够在正确的时间抓住用户并从其设备获得意外的批准。同样，如果你不断用多因素批准骚扰用户，他们可能会因反复拒绝而放弃并批准你的请求。

作为渗透测试人员，作者可以证明这些策略在真实评估中是有效的。我们曾遇到一起案例，用户认为其 MFA 令牌出了问题，因为反复提示批准登录。受影响的用户联系了其 IT 部门，后者迅速在尝试排查问题时禁用了 MFA。没有人似乎在意登录尝试的 IP 地理位置位于全国另一边的城市，但这为报告增添了精彩内容。

虽然这些不应该被期望在每次测试中都成为轻松的胜利，但这些例子来自作者在测试中观察到的真实案例。因此，即使你遇到 MFA，仍然有希望！

正如你将在本书后面看到的那样，在 Azure 环境中通常有大量提升权限的选项，但经常，你需要从一个权限较低的账户开始——也许是一个密码选择不当且未注册 MFA 的账户。

## 总结

在本章中，我们从外部人员几乎没有目标云资产信息的角度，探讨了如何应对 Azure 渗透测试场景。我们还学习了如何查找暴露资源的配置错误漏洞，这可能导致对租户的初始访问。

除此之外，我们还介绍了用于进一步利用的各种工具和技术，以攻破 Azure AD 用户账户。本章讨论的信息有望使你能够匿名发现和攻击公共 Azure 资源和易受攻击的云身份。

在下一章中，我们将介绍在你猜测到凭据或获得对订阅的 Reader 级别访问后可以进行的攻击。

## 进一步阅读

要了解本章所涵盖内容的更多信息，请查看以下资源：

- Azure AD Identity Protection: https://docs.microsoft.com/en-us/azure/active-directory/identity-protection/overview-identity-protection
- Azure AD Password Protection: https://docs.microsoft.com/en-us/azure/active-directory/authentication/concept-password-ban-bad
- Social engineering: https://www.csoonline.com/article/2124681/what-is-social-engineering.html
- *Implementing Microsoft Azure Security Technologies by David Okeyode, Packt Publishing*

## 第 2 部分：对 Azure 的经过身份验证的访问

本书的第二部分将介绍在 Azure 环境中经过身份验证的用户可用的核心攻击。它们将按每个攻击所需的一般订阅权限（Reader、Contributor、Owner）进行划分。本节还解释了攻击者在获得特权 Azure AD 账户访问权限后可用的持久化方法。

本书的这一部分包含以下章节：

- 第 4 章，滥用 Reader 权限
- 第 5 章，滥用 IaaS 服务上的 Contributor 权限
- 第 6 章，滥用 PaaS 服务上的 Contributor 权限
- 第 7 章，滥用 Owner 和特权 Azure AD 角色权限
- 第 8 章，在 Azure 环境中持久化

## 滥用 Reader 权限

虽然 Reader 角色在订阅中的使用不如 Contributor 或 Owner 角色频繁，但它确实允许用户读取有关服务资源和配置的基本信息。作为进入环境的初始入口点，Reader 角色可能允许你读取可用于转向更特权角色的敏感信息。

Reader 角色不允许对服务或资源进行任何修改，但它会允许攻击者枚举环境的攻击面。这是经常为 Azure 渗透测试期间可能配置的任何 Azure AD 账户发放 Reader 访问权限的驱动原因。

出于充分的理由，许多组织希望避免在渗透测试期间向渗透测试人员授予订阅上的可变异（Contributor 或更高）访问权限。通过发放 Reader 角色账户，测试人员将获得洞察力，帮助他们识别从匿名角度可能不明显的配置错误。

