

^([7](#ch10.html#id880-marker)) 如果你遇到 `ServiceException` 或权限错误，运行 `gcloud auth login` 并按照步骤重新进行身份验证。身份验证完成后，请确保重新运行命令以完成上传。

^([8](#ch10.html#id881-marker)) 运行 `ls -l` 以确认指定名称的文件存在于当前目录中。

^([9](#ch10.html#id883-marker)) 在我最近一次运行动手示例时，生成的文件名达到了 311 字节。

^([10](#ch10.html#id885-marker)) 此端点可通过 http://169.254.169.254/ 访问，并且可供在 Compute Engine、Cloud Run 以及其他可访问元数据服务器的 Google Cloud 管理资源上运行的应用程序和工作负载使用。

^([11](#ch10.html#id886-marker)) 如果 `INPUT_BUCKET` 变量已设置，可以跳过此步骤。

^([12](#ch10.html#id887-marker)) 如果在上传文件时遇到权限错误，请运行 `gcloud auth login` 并按照步骤重新进行身份验证。

^([13](#ch10.html#id888-marker)) 即使由于 Cloud Resource Manager API 未为项目启用而收到 `PERMISSION_DENIED` 错误，你仍应能够从响应中获取项目编号。

^([14](#ch10.html#id889-marker)) 使用项目 ID 而不是项目编号也应该可以正常工作。

^([15](#ch10.html#id890-marker)) 电子邮件地址应以 `html-to-pdf` 开头。

^([16](#ch10.html#id891-marker)) 每个服务帐户对可拥有的活动密钥数量都有最大限制。达到此限制后尝试创建新密钥将导致 `FAILED_PRECONDITION` 错误。

^([17](#ch10.html#id899-marker)) 请参阅 [Google Cloud 文档](https://oreil.ly/c-_6B) 了解更多详情。

^([18](#ch10.html#id900-marker)) 你可以通过导航到 Google Cloud 控制台中的凭据页面，查看 API 密钥列表，并根据需要更新或删除密钥来完成此操作。

# 第 11 章. Hacking and Securing Azure Functions

在过去的几章中，你探索了攻击者如何利用 AWS 和 Google Cloud 上运行的无服务器应用程序中的漏洞和错误配置。你检查了未应用最小权限原则的后果，并亲身体验了过于宽泛的权限如何使攻击者能够提升权限、窃取敏感数据、设置后门并破坏整个云帐户。通过模拟真实世界的攻击和防御场景，你学习了如何识别错误配置、评估防御措施并应用各种策略来保护在 AWS 和 Google Cloud 上运行的无服务器应用程序。

在本章中，你将把重点转移到 Microsoft Azure，并探索攻击者如何滥用云平台上运行的无服务器应用程序中的错误配置和漏洞。如同前几章一样，你将准备一个有意设计为易受攻击的无服务器应用程序，使用不安全的默认设置和错误配置进行配置，以便你可以安全地模拟攻击者利用各种漏洞和安全问题。你将首先设置一个易受攻击的 Azure 函数，然后继续模拟攻击者利用该函数中的代码注入漏洞。为了加快安全测试模拟的速度，你将准备并使用一个自定义脚本，该脚本可自动化向易受攻击函数的调用端点发送自定义恶意负载、捕获并返回函数的响应，并允许你通过读取-求值-打印循环（REPL）进行交互式迭代，如 [Figure 11-1](#ch11.html#ch11-complete-setup) 所示。

![](assets/lsse_1101.png)

你还将把密钥存储在 Azure Key Vault 中，并探索代码注入漏洞与过于宽松的管理身份结合时，攻击者如何利用其窃取这些密钥。你会看到，虽然使用 Azure Key Vault 等密钥管理器服务被认为是一种最佳实践，但它并非万无一失，因为配置不当的身份或过于宽泛的权限仍可能允许攻击者窃取保存在保管库中的凭据和其他敏感密钥。

本章将涵盖以下内容：

* [“Setting Up and Deploying a Vulnerable-by-Design Serverless Function”](#ch11.html#setting_up_and_deploying_a_vulnerable-by-design_serverless_function_ch11-hacking-and-securing-azure-functions_1762890051754)
* [“Exploiting the Vulnerable Serverless Function”](#ch11.html#exploiting_the_vulnerable_serverless_function_ch11-hacking-and-securing-azure-functions_1762890066699)
* [“Storing Secrets in Azure Key Vault”](#ch11.html#storing_secrets_in_azure_key_vault_ch11-hacking-and-securing-azure-functions_1762890081359)
* [“Abusing an Overly Permissive Managed Identity to Exfiltrate Key Vault Secrets”](#ch11.html#abusing_an_overly_permissive_managed_identity_to_exfiltrate_key_vault_secrets_ch11-hacking-and-securing-azure-functions_1762890095111)

到本章结束时，你将更好地具备识别 Azure 上无服务器应用程序中的错误配置和漏洞的能力，重现攻击者如何利用它们，并开发自定义脚本和工具来加速和自动化安全测试和验证。

# Reviewing Technical Prerequisites

对于本章，你将使用两个 Azure 帐户：Serverless Lab 帐户用于托管易受攻击的按设计实验室环境，以及 Attacker 帐户用于模拟攻击和测试安全控制。强烈建议使用全新的帐户且没有任何现有资源，以最大程度地减少安全相关副作用并确保实验室环境按预期运行。保持两个帐户在单独的浏览器会话（或隐身/隐私窗口）中打开，以避免会话冲突。

###### Note

为了帮助你完成练习和模拟，本章中使用的代码和命令的副本可在 [GitHub gist (see `ch11.md`)](https://oreil.ly/0mSPU) 中获得。

# Setting Up and Deploying a Vulnerable-by-Design Serverless Function

在 [Chapter 2](#ch02.html#chap-02-title) 中，你探索了实现无服务器运营模式的各种实现模式、架构和云服务。你了解到，一旦你理解了无服务器设计的 fundamentals，你就可以轻松地将这些概念映射到 Azure 等云平台。例如，如果你在 Azure 上构建无服务器 Web 应用程序或 API，你将使用 Azure Functions 处理计算任务，使用 Azure Blob Storage 存储应用程序数据和资产，并使用 Azure API Management、Event Grid 和 Service Bus 等支持服务来管理集成和事件驱动的工作流。此外，你还将使用 Azure Key Vault 安全地存储和管理密钥、凭据和其他敏感配置值。虽然这些服务处理了大部分运营复杂性，但正确保护它们仍然是你的责任，因为错误配置可能导致数据泄露、未经授权的访问或无服务器应用程序的完全入侵。同时，了解攻击者如何发现和利用这些错误配置对于确保你的无服务器应用程序免受真实世界威胁的攻击至关重要。

在本节中，你将主要关注 Azure Functions，并设置一个易受攻击的按设计无服务器函数，以便你探索攻击者如何发现和利用代码注入等漏洞来破坏函数并窃取源代码、硬编码密钥和环境变量。为了简化本章的模拟，我们不会实现前端界面，而是完全专注于无服务器函数，如 [Figure 11-2](#ch11.html#complete-setup-focus-function) 所示。

![](assets/lsse_1102.png)

