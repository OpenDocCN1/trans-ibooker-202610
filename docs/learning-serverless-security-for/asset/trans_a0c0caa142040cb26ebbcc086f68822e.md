

保护无服务器应用程序从定义谁可以在什么条件下做什么开始——在任何代码部署之前。这最终通过 `IAM` 的配置方式来强制执行。在 AWS 中，每个资源都依赖 `IAM` 进行访问控制。所有 AWS 资源，例如 `AWS Lambda` 函数、`Amazon S3` 存储桶和 `Amazon DynamoDB` 表，都依赖 `IAM` 策略来定义和强制执行访问权限。由于 `IAM` 管理着 AWS 中几乎每个操作和资源的访问权限，因此了解其工作原理对于保护你的无服务器环境并防止意外的权限提升或数据泄露至关重要。

让我们开始探索 `IAM` 的核心概念以及 AWS 如何定义身份和权限。AWS `IAM` 的核心是以下关键身份类型：

### User
具有长期凭证的个人身份，例如用户名和密码或访问密钥。这些用户通常与需要直接与 AWS 服务交互的人员或应用程序相关联。例如，一名读取和写入 S3 存储桶、管理 Secrets Manager 密钥、与 Amazon Bedrock 交互并部署 Lambda 函数的开发人员将被分配具有适当权限的 `IAM` 用户。

### Group
一组用户的集合，可作为一次性为多个用户应用权限的方式。例如，Developers 组可能包含团队中的所有开发人员，并且你可以授予他们部署 Lambda 函数、读取和写入 S3、管理 Secrets Manager 密钥以及与 Amazon Bedrock 交互的权限。通过这种方法，而不是为每个开发人员单独分配权限，你可以通过组集中管理权限。

### Role
具有一组定义权限的身份，受信任实体（如 AWS 服务、应用程序或来自另一个账户的用户）可以 assume。该身份。例如，Lambda 函数可以被分配一个执行角色，该角色授予其在执行期间访问 S3 存储桶、DynamoDB 表或其他 AWS 资源所需的权限。与 `IAM` 用户不同，角色没有长期凭证。相反，它们依赖于通过 AWS 安全令牌服务 (`STS`) 动态颁发的短期凭证。

这些身份默认情况下没有任何权限。访问权限是通过附加策略授予的，策略是 JavaScript Object Notation (`JSON`) 文档，指定在特定 AWS 资源上允许或拒绝哪些操作。你可以将 `IAM` 策略视为安全建筑中的数字门禁卡。每张门禁卡（策略）指定某个人（身份）可以进入哪些房间（资源）以及进入后可以做什么。没有门禁卡，门默认保持锁定。正如建筑安全取决于谁有权访问哪些房间一样，AWS 安全也依赖于你如何仔细管理和审计 `IAM` 权限。

AWS 有几种类型的策略：

### Managed policies
可独立附加到用户、组或角色的 `IAM` 策略。它们分为两种类型：AWS 管理的策略，由 AWS 为常见用例预定义；以及客户管理的策略，由你创建以强制执行特定于环境的细粒度权限。

### Inline policies
直接嵌入到特定 `IAM` 用户、组或角色中的策略。

### Resource-based policies
直接附加到 AWS 资源（如 S3 存储桶、Lambda 函数或 SQS 队列）的策略。这些策略指定哪些用户、角色或账户可以访问资源以及它们被允许执行的确切操作。

这些 `IAM` 身份和策略共同构成了管理并控制对 AWS 资源访问的核心框架。正如建筑可能使用不同种类的门禁卡（有些在部门间共享，有些分配给个人，还有一些与特定房间绑定）一样，AWS 使用托管、内联和基于资源的策略在不同级别控制访问。每种策略类型都在确保仅在需要的地方并且根据明确定义的规则授予访问权限方面发挥着特定作用。

### Note
本章不会详细介绍 AWS `IAM`。要了解更多信息，请查看 AWS re:Inforce 2023 会议 [“A First-Principles Approach: AWS Identity and Access Management (IAM) (IAM201)”](https://oreil.ly/CGt-k)。

在我们深入本章的动手部分之前，让我们也定义并解释一些与你在即将到来的部分中将使用的内容相关的关键术语、概念和服务：

### Authentication
确认用户、应用程序或服务的身份的过程，以确保它们就是它们声称的那样。

### Authorization
确定经过身份验证的用户、应用程序或服务被允许对特定资源执行哪些操作的过程。

### Amazon Resource Name (ARN)
AWS 资源的全局唯一标识符，指定资源类型、区域、账户以及名称或 ID。这使得在定义权限或进行 API 请求时能够准确引用资源成为可能。

### AWS Security Token Service (STS)
提供用户或应用程序的临时、短期凭证的服务。这使得无需依赖长期凭证即可安全地访问 AWS 资源。

### Root user
你在首次注册 AWS 时创建的原始账户。根用户对账户中的所有资源和服务具有完全的管理员访问权限。

了解 AWS `IAM` 至关重要，因为它不仅决定谁可以访问你的 AWS 资源以及他们可以执行哪些操作，还能让你识别攻击者可能利用的错误配置或过于宽松的设置。正确配置 `IAM` 是不可协商的，因为配置错误的权限可能会允许未经授权的用户修改资源、访问密钥或在你的 AWS 环境中提升权限。

有了这些概念，我们开始设置我们的实验室环境。

# Setting Up the Vulnerable-by-Design Serverless Lab Environment

理解安全在你能够安全地尝试真实攻击时变得更容易。诸如配置错误的权限或暴露的凭证之类的概念在你亲眼看到它们实际运行之前是抽象的。使用有意设计为易受攻击的实验室环境可以让你在受控环境中安全地进行实验、探索配置错误的影响并加深对无服务器安全的理解。

在本章中，你将设置一个易受攻击的 AI 驱动无服务器应用程序，该应用程序具有简单的聊天 UI，可通过前端代码中硬编码的 AWS 凭证连接到 Amazon Bedrock 运行时 API，如 [Figure 4-1](#ch04.html#vulnerable-serverless-ai-application) 所示。

![](assets/lsse_0401.png)

由于这些凭证直接属于附加了 `AdministratorAccess` 策略的 `IAM` 用户，任何获得这些凭证访问权限的人都可以完全控制 AWS 账户，包括创建、修改或删除资源。因此，此易受攻击的无服务器应用程序仅 intended 在你的本地机器上运行，并且绝不应部署到公开可访问的环境（例如 S3 主持的静态网站）以避免将 AWS 凭证暴露给公众。

设置过程分为以下几个部分：

* [“Creating an IAM User with the AdministratorAccess Policy Attached”](#ch04.html#creating_an_iam_user_with_the_administratoraccess_policy_attached_ch04-exploiting-and-securing-exposed-credentials_1762886606144)
* [“Creating Trails in CloudTrail to Capture API Calls Made in the Account”](#ch04.html#creating_trails_in_cloudtrail_to_capture_api_calls_made_in_the_account_ch04-exploiting-and-securing-exposed-credentials_1762886629218)
* [“Configuring Bedrock Model Invocation Logging”](#ch04.html#configuring_bedrock_model_invocation_logging_ch04-exploiting-and-securing-exposed-credentials_1762886658804)

