
要对文件进行签名，`Set-AuthenticodeSignature` cmdlet 要求你提供一个有效的代码签名证书。大多数认证机构收费提供 Authenticode 代码签名证书。通过使用来自信誉良好的认证机构（如 VeriSign 或 Thawte）的 Authenticode 代码签名证书，你可以确保所有用户都能验证脚本上的签名。一些在线服务提供极其便宜的代码签名证书，但请注意，许多计算机可能无法验证由这些证书创建的数字签名。

### 注意

你仍然可以通过生成自己的代码签名证书，在自己的计算机上获得代码签名的许多好处。虽然其他计算机将无法识别该签名，但它仍然能在你自己的计算机上提供防篡改保护。有关此方法的更多信息，请参阅 [Recipe 18.5](#ch18.xhtml#create_a_self-signed_certificate)。

`-TimeStampServer` 参数允许你以一种方式对脚本或格式文件进行签名，使得脚本或格式文件上的签名在你的代码签名证书过期后仍然有效。

有关 `Set-AuthenticodeSignature` cmdlet 的更多信息，请输入 `Get-Help Set-AuthenticodeSignature`。

## 另请参阅

[Recipe 18.5](#ch18.xhtml#create_a_self-signed_certificate)

# 18.5 创建自签名证书

## 问题

你想要为测试目的创建代码签名、文档加密或 SSL 证书。

## 解决方案

使用 `New-SelfSignedCertificate` cmdlet。

要创建适用于 PowerShell 脚本签名的证书：

```
## Generate the self-signed certificate (requires an administrative console)
New-SelfSignedCertificate -DnsName 'guide@leeholmes.com' -Type CodeSigningCert
$cert = dir cert:\LocalMachine\My -DnsName 'guide@leeholmes.com' -CodeSign

## Make it trusted on this machine
$caCert = Get-ChildItem cert:\LocalMachine\CA -DnsName 'guide@leeholmes.com'
$caCert | Move-Item -Destination Cert:\LocalMachine\Root

## Sign the file
Set-AuthenticodeSignature .\SignedScript.ps1 $cert
```

要创建适用于加密消息语法 (CMS) cmdlet 的证书：

```
## Generate the self-signed document encryption certificate (requires an
## administrative console)
New-SelfSignedCertificate -DnsName 'guide@leeholmes.com'
    -Type DocumentEncryptionCertLegacyCsp
$cert = Get-ChildItem cert:\localmachine\my -DocumentEncryptionCert

## Encrypt the message
"Hello World" | Protect-CmsMessage -To $cert
```

## 讨论

无需购买官方代码签名证书，即可享受已签名脚本的防篡改保护功能。你可以通过创建自签名证书来实现这一点。使用自签名证书签名的脚本在其他计算机上将不被识别为有效，但你仍然可以在自己的计算机上对其进行签名和使用。

当 `New-SelfSignedCertificate` 运行时，它默认会生成两个证书：一个测试颁发证书颁发机构和一个由其生成的证书（你的签名证书）。当 Windows 验证数字签名时，颁发证书颁发机构（或其颁发者之一）必须位于证书存储中的“受信任的根 CA”位置。因此，为了使我们的证书在我们的计算机上受信任，我们将测试证书颁发机构移动到受信任的根存储中。

### 注意

如果你希望使用密码或生物识别证明来保护证书，可以使用 `New-SelfSignedCertificate` 的 `-KeyProtection` 参数。这将防止其他应用程序代表你使用你的密钥。

我们使用非常相似的过程来生成文档加密证书。由于文档加密证书不需要来自受信任的根颁发机构，我们可以跳过使其在我们的计算机上受信任的步骤。

有关运行脚本的更多信息，请参阅 [Recipe 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

## 另请参阅

[Recipe 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

# 18.6 在企业中管理 PowerShell 安全性

## 问题

你想要在企业环境中控制 PowerShell 的安全功能。

## 解决方案

你有两种方法可以在整个企业范围内管理 PowerShell 的安全功能：

*   应用 PowerShell 的组策略模板，通过组策略控制 PowerShell 的执行策略。
*   部署 Microsoft 证书服务，为域账户自动生成 Authenticode 代码签名证书。

## 讨论

无论单独使用还是组合使用，这些功能都能让你自定义整个域中的 PowerShell 环境。

### 应用 PowerShell 的组策略模板

Windows PowerShell 的管理模板允许你在计算机级别和用户级别覆盖计算机的本地执行策略首选项。Windows PowerShell 的管理模板默认包含在 Windows 中。你可以在 PowerShell Core 的 `$PSHome` 安装目录中找到其管理模板。要使用它们，请将 ADMX 文件复制到 `C:\Windows\Policy​Definitions`，将 ADML 文件复制到 `C:\Windows\PolicyDefinitions\en-US`，或者使用 `InstallPSCorePolicyDefinitions.ps1` 脚本为你完成此操作。

### 注意

尽管组策略设置会覆盖本地首选项，但 PowerShell 的执行策略不应被视为一种保护系统免受用户侵害的安全措施。它是一种有助于防止不受信任的脚本在系统上运行的安全措施。正如 [Recipe 18.1](#ch18.xhtml#enable_scripting_through_an_execution_po) 中所述，PowerShell 只是一个载体，允许用户执行他们已拥有的 Windows 权限所允许的操作。

要编辑组策略设置，请启动组策略对象编辑器 MMC 管理单元。组策略编辑器 MMC 管理单元在其管理模板节点下提供 PowerShell 作为一个选项，如 [图 18-3](#ch18.xhtml#powershell_group_policy_configuration) 所示。你可以在管理模板树的根目录下找到 PowerShell Core，在 Windows 组件下找到 Windows PowerShell。

![](Images/wps4_1803.png)

默认状态为“未配置”。在此状态下，PowerShell 从计算机的本地首选项获取其设置（如 [Recipe 18.1](#ch18.xhtml#enable_scripting_through_an_execution_po) 所述）。如果将状态更改为“已启用”选项之一（或“已禁用”），PowerShell 将使用此配置，而不是计算机的本地首选项。

### 注意

无论何种情况，PowerShell 都会遵守这些组策略设置。这包括计算机管理员可能认为会降低安全性的设置——例如，Unrestricted 组策略覆盖 AllSigned 本地首选项。

每用户组策略设置覆盖计算机的本地首选项，而每计算机组策略设置覆盖每用户设置。

### 部署 Microsoft 证书服务

虽然这超出了本书的范围，但 Microsoft 证书服务允许你自动向任何或所有域用户部署代码签名证书。这提供了显著的好处，因为它有助于保护用户免受意外或恶意的脚本篡改。

有关此主题的介绍，请访问 [Microsoft 文档站点](http://technet.microsoft.com) 并搜索“Enterprise Design for Certificate Services”。有关脚本签名的更多信息，请参阅 [Recipe 18.4](#ch18.xhtml#sign_a_powershell_script_or_formatting_f)。

## 另请参阅

[Recipe 18.1，“通过执行策略启用脚本”](#ch18.xhtml#enable_scripting_through_an_execution_po)

[Recipe 18.4，“对 PowerShell 脚本、模块或格式文件进行签名”](#ch18.xhtml#sign_a_powershell_script_or_formatting_f)

# 18.7 按发布者、路径或哈希阻止脚本

## 问题

除了 PowerShell 的执行策略外，你还想要阻止或审核系统上运行的脚本和应用程序。
