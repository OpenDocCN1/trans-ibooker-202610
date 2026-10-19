
一个 `chatbot` 是一种通过文本或语音与用户交互的应用程序。如今，许多聊天机器人使用 LLM 来理解问题并提供更智能、更像人类的响应。

AWS CloudShell 是一个基于浏览器的 shell 环境，让您可以安全地直接从 AWS 管理控制台运行 AWS CLI 命令、脚本和工具。

如果 AWS 管理控制台在您的屏幕上看起来有点不同，请不要感到惊讶。AWS 会定期更新界面以提高可用性并添加新功能。虽然布局可能会发生变化，但此处描述的功能保持不变。

您可以在搜索框中输入 `admin` 来定位 `AdministratorAccess` 策略。

或者，您可以从账户中的 IAM 用户列表中定位 `super-admin-user` IAM 用户。

您可以将这些值暂时存储在代码编辑器中。

访问密钥由访问密钥 ID 和秘密访问密钥组成，为 IAM 用户提供长期凭证。相比之下，临时安全凭证（例如由 AWS STS 发出的凭证）包含额外的会话令牌，并且会在有限的时间后自动过期。

AWS Key Management Service (KMS) 是一种托管服务，让您可以创建和控制加密密钥来保护您的数据。它安全地存储密钥，并与其他 AWS 服务协同工作，以便轻松加密和解密数据。

管理事件记录控制平面操作，例如创建、删除或修改资源。数据事件捕获资源本身的活动，例如 S3 对象级操作或 Lambda 函数调用。洞察事件提供异常检测，突出 API 活动中的不寻常模式。启用这些功能可以增强您对管理操作和资源级使用的可见性。

您将在“禁用 CloudTrail 日志以规避检测”中更详细地了解其运作方式。

Amazon CloudWatch `log group` 是日志流的集合，用于组织和存储来自 AWS 资源或应用程序的日志数据。这使您能够以集中方式监控、搜索和分析事件，类似于使用文件柜将文件存放在抽屉和文件夹中，以便在需要时轻松查找和查看特定记录或信息。

或者，您可以点击 AWS 管理控制台顶部导航栏中的 `>_` 来打开 CloudShell 终端。

Amazon S3 静态网站托管让您可以使用 S3 存储桶通过 HTTP 提供静态文件，如 HTML、CSS、JavaScript 和图像，从而无需管理服务器即可托管网站。

您可以在本地运行命令之前导航到组织和管理项目文件的目录。

`tree` 命令可快速查看项目的目录结构。为了避免深层嵌套目录带来的杂乱，运行 `tree -L 2` 可获得更清晰的视图，因为它将显示限制为仅两层深。

该文件应类似于 `/assets/index-abc-def.js`，尽管确切字符可能有所不同。

您还可以查找其他常见关键字，例如 `aws_access`、`credentials`、`awsKey`、`awsSecret`、`accessKey` 或 `secretAccess`，以在前端代码中发现密钥和凭证。

这些命令适用于安装了 `wget` 的 macOS 和 Linux 系统。Windows 用户可能需要使用 WSL 或替代方法来运行这些命令。

虽然这些搜索表达式旨在匹配 AWS 凭证模式，但它们也可能返回与模式匹配但并非实际凭证的字符串。

或者，您可以点击 AWS 管理控制台顶部导航栏中的 `>_` 来打开 CloudShell 终端。

这假设没有其他显式拒绝、权限边界或其他限制。

出于简单起见，此示例仅列出 `us-east-1` 区域中的跟踪。实际上，攻击者（以及执行完整发现的防御者）会枚举所有区域中的 CloudTrail 跟踪，因为跟踪可以按区域或账户范围配置。

在运行命令之前，请确保将 `[S3 BUCKET NAME]` 替换为存储日志文件 S3 存储桶的实际名称。

您可以在继续下一节之前选择重新启用 CloudTrail 日志，以便您可以检查和分析本章生成的活动日志。使用 `aws cloudtrail start-logging` 命令重新启用现有跟踪的日志记录。

CloudTrail 日志可以通过使用服务控制策略、S3 对象锁定和组织跟踪等控制措施来保护。这些机制有助于强制执行不可变性并限制即使拥有 `AdministratorAccess` 的用户也无法禁用或删除日志。

您可以将这些值暂时存储在代码编辑器中。

您可以在聊天 Web 应用程序的源代码中找到模型 ID。对于此步骤，您可以使用 `super-admin-user-2` 或 `target-account` 配置文件来调用 Bedrock 模型。

您可能会收到以下 `ValidationException` 消息：`An error occurred (ValidationException) when calling the InvokeModel operation: Invocation of model ID anthropic.claude-3-5-sonnet-20240620-v1:0 with on-demand throughput isn’t supported. Retry your request with the ID or ARN of an inference profile that contains this model`。这发生是因为 AWS 现在强制使用推理配置文件，该配置文件定义了模型吞吐量在每个区域内的预配和共享方式。

您的输出可能与此示例不完全匹配，因为模型响应可能有所不同。此外，如果您的提示被标记为潜在恶意，可能会被拒绝。

当然，如果攻击者获得了有效的 AWS 凭证，他们仍可直接通过 CLI 或 SDK 调用 Bedrock 模型，并绕过 Lambda 函数使用的任何防护措施。

如果 CloudShell 终端已打开，您可以跳过此步骤。

您可以使用 `ls unzipped` 列出提取的文件。

本节中的动手步骤在 IPython 8.18.1 上进行了测试。

本节中的动手步骤在 pandas 2.3.2 上进行了测试。

您可能会得到不同的行值集。此外，您可以配置 pandas `pd.set_option("display.max_columns", None)` 以显示所有可用列。

请注意，您可能会得到不同的 eventType 值集，例如 `'AwsCloudTrailInsight'`、`'AwsApiCall'` 和 `'AwsConsoleAction'`。

或者，您可以使用 `print(list(invoke_model_events.columns))`。

您可以使用 `merged_df.head()` 快速预览前几行合并数据并验证连接是否按预期工作。

您可能会得到略有不同的值集，具体取决于 CloudTrail 和调用日志中捕获的特定事件。

# 第 5 章. 利用和保护配置错误的 AWS IAM 角色
