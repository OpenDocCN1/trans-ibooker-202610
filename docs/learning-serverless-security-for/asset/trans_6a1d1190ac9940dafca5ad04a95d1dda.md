

在本章中，你将探索攻击者如何利用公开暴露的 Lambda URL 并在函数的执行环境中运行任意代码。你将研究 Lambda 函数中的代码注入漏洞，当与过于宽松的 IAM 角色结合时，如何让攻击者窃取存储在 secrets manager 服务中的密钥。

为了帮助你亲身体验攻击者的视角，本章被组织为五个部分：

*   [“准备和部署一个易受攻击的无服务器 Web 应用程序”](#ch06.html#preparing_and_deploying_a_vulnerable_serverless_web_application_ch06-hacking-publicly-accessible-lambda-functions_1762887997958)
*   [“测试常见的 S3 存储桶错误配置”](#ch06.html#testing_for_common_s3_bucket_misconfigurations_ch06-hacking-publicly-accessible-lambda-functions_1762888018450)
*   [“使用代码注入攻击在 Lambda 函数中执行任意代码”](#ch06.html#using_code_injection_attacks_to_execute_arbitrary_code_in_lambda_functions_ch06-hacking-publicly-accessible-lambda-functions_1762888039235)
*   [“通过代码注入和出站网络访问窃取敏感数据”](#ch06.html#exfiltrating_sensitive_data_via_code_injection_and_outbound_network_access_ch06-hacking-publicly-accessible-lambda-functions_1762888055692)
*   [“通过易受攻击的 Lambda 函数窃取 Secrets Manager 密钥”](#ch06.html#stealing_secrets_manager_secrets_through_a_vulnerable_lambda_function_ch06-hacking-publicly-accessible-lambda-functions_1762888073588)

到本章结束时，你将更好地具备在攻击者利用之前识别易受攻击的无服务器配置的能力。

# Reviewing Technical Prerequisites

要跟随本章学习，你需要以下内容：

*   两个 AWS 账户：一个用于设置和托管易受攻击的无服务器实验室环境（Serverless Lab 账户），另一个用于运行模拟攻击（Attacker 账户）。确保两个账户都已准备就绪，并在单独的浏览器会话（或隐身/隐私窗口）中打开，以避免会话冲突。
*   在本地机器上安装的代码编辑器（如 VS Code 或 Sublime Text）。

如果你使用 Windows，请确保你有运行 macOS 或 Linux 命令的方法——例如，使用 WSL，或带有 Linux 发行版的 VM。

> Note
>
> 为了帮助你完成练习和模拟，本章中使用的代码和命令的副本可在 [GitHub gist (see `ch06.md`)](https://oreil.ly/0mSPU) 中获得。

# Preparing and Deploying a Vulnerable Serverless Web Application

代码注入是无服务器应用程序中远程代码执行的主要原因之一。它允许攻击者运行任意代码、访问敏感凭据或配置、通过网络外泄数据，并更改函数的行为或响应。当与过于宽松的 IAM 角色结合时，单个代码注入漏洞可能会让攻击者利用函数的权限并访问其他云资源。

在本节中，你将设置一个易受攻击的无服务器 Web 应用程序，以探索代码注入攻击如何导致 AWS Lambda 中未经授权的代码执行。这将帮助你更深入地了解不安全的代码以及函数级错误配置如何使无服务器应用程序暴露于各种安全威胁。

本章中你将使用的无服务器 Web 应用程序由托管在 Amazon S3 存储桶中的静态 Web 前端组成，该前端向一个故意容易受到代码注入攻击的 Lambda 函数发出请求（[Figure 6-1](#ch06.html#lab-setup-00)）。

![](assets/lsse_0601.png)

此应用程序允许用户在浏览器中输入数学表达式，后端对其进行求值并将响应发送回前端。在你完成动手示例的过程中，你将了解这些问题和错误配置如何被利用，并将看到可以实施的实用对策来减轻这些风险。

> Caution
>
> 在完成本章和下一章中的动手示例和模拟后，请务必删除云账户中所有的 Lambda 函数、S3 存储桶、IAM 角色和其他资源，以避免意外的费用和安全风险。

## Setting Up a Vulnerable AWS Lambda Function

让我们开始设置一个易受攻击的 AWS Lambda 函数，该函数接受并求值用户提供的数学表达式。在本节中，你将重新创建并探索在真实无服务器应用程序中发现的几个编码和配置问题，包括函数代码中使用 `eval()`、源代码中硬编码的凭据以及存储在环境变量中的密钥。你还将使 Lambda 函数通过未经身份验证的函数 URL 暴露，该 URL 由托管在 S3 存储桶中的静态 Web 前端直接调用。^[1](#ch06.html#id659)

在深入动手部分之前，让我们定义与本章相关的几个关键术语：

**AWS Lambda function**

这段自包含的代码是无服务器应用程序的基本构建块。它响应特定事件运行，例如 API 请求、文件上传或数据库更新，并在 AWS 管理的安全隔离环境中执行。^[2](#ch06.html#id661) 每个函数都有一个单一的处理程序方法，该方法接收事件数据并执行你指定的逻辑。

**Lambda execution environment**

这个安全隔离的空间管理运行函数代码的资源。你在创建函数时提供配置——例如内存和超时——Lambda 使用此配置来配置执行环境。

**Lambda runtime**

管理调用事件流、上下文信息和函数响应的语言特定环境。Lambda 函数的运行时作为进程在 Lambda 执行环境内部运行。

**Lambda execution role**

函数在运行时假定的 AWS IAM 角色。它定义了函数可以执行的操作以及可以操作的资源。

**Function URL**

一个 HTTPS 端点，允许你直接从 Web 浏览器、`curl` 或任何 HTTP 客户端调用 Lambda 函数，而无需 API 网关。它提供了一种快速简单的方式来公开你的函数，非常适合原型设计和构建小型或简单应用程序。

你可以将本节中将设置的易受攻击 Lambda 函数视为高安全建筑内的维护控制台。它旨在让授权人员执行有限的系统任务，但如果未受保护或配置错误，攻击者可以将其用作进入点来篡改或控制建筑的系统。

> Note
>
> 本节假设你使用的是 Serverless Lab 账户，该账户托管易受攻击的无服务器实验室环境。

让我们现在逐步进行设置过程：

1. Sign in to the [AWS Management Console](https://oreil.ly/qe5es).
2. 在搜索栏中输入 `lambda`，然后从搜索结果列表中选择 Lambda。
3. 在左侧导航窗格中，选择 Functions，然后单击“Create function”。

   > Caution
   >
   > 在本章中，所有示例都假设你的 AWS 区域设置为 `us-east-1`。请确保 AWS 控制台右上角选择的区域设置为 `United States (N. Virginia)`。

4. 在“Create function”页面上，选择“Author from scratch” ([Figure 6-2](#ch06.html#author-from-scratch))。

   ![](assets/lsse_0602.png)

   在“Basic information”窗格中，在“Function name”字段中指定 `lambda-0010`。然后为 Runtime 选择 Python 3.13。

   > Tip
   >
   > 如果你在阅读本文时有更新的 Python 运行时可用——例如 Python 3.14 或更高版本——你可以选择它。

