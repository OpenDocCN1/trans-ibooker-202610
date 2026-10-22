

一旦前端直接将文件上传到 S3 而不是通过 Web API 发送，添加 S3 存储桶的触发器就可以直接在管理控制台中，在 S3 存储桶配置的 Properties 选项卡上完成，如图 3-15 所示。

![](img/044e6b5616cc4c02f2683d0cc9990365_123_0.png)

图 3-15. 向 S3 存储桶添加事件通知

将 Step Functions 状态引擎添加为此事件的目的地，我们就得到了最终形式的事件驱动、无服务器 C# 简历上传服务！最终架构如图 3-16 所示。

www.example.com

# Welcome to our Website!

Upload your resume here

Upload File...

PDF File

S3 Bucket

s3:ObjectCreated Event

Step Functions

Textract

Lambda

Email

图 3-16. 我们简历上传示例的最终事件驱动架构

正如你所见，我们现在拥有一个简洁、描述性强且事件驱动的系统，其动态性足以让我们在不冒着在其他地方引入错误或问题的风险的情况下扩展或修改工作流的各个部分。最后需要考虑的是，我们如何管理所有这些 moving parts 并在同一位置轻松配置它们。

## Serverless Application Model (SAM)

到目前为止，我们一直通过登录 AWS 管理控制台并在 UI 中点击来完成所有配置更改。这对于实验来说是可以的，但并不是特别适合运行生产系统的方法。一次错误的点击就可能导致应用程序的一部分出现中断。这就是基础设施即代码（IaC）可以提供帮助的地方。

基础设施即代码是通过机器可读的定义文件配置你的无服务器基础设施的过程。CloudFormation 是 AWS 中使用的 IaC 工具，允许你将整个云配置保存在 YAML 或 JSON 文件中。你 AWS 中的几乎所有内容都可以在 CloudFormation 模板中建模，从 DNS 设置到 S3 存储桶属性再到 IAM 角色和权限。当需要更改设置时，你只需更改 CloudFormation 模板中的值并告诉 AWS 将更改应用到你的资源。最明显的好处是，你可以将模板 JSON/YAML 文件检入版本控制，并像对任何其他代码更改一样，对每次更改进行代码审查、审计，并在沙箱/暂存环境中进行测试。

然而，CloudFormation 的一个缺点是它可能相当复杂且冗长。由于可以在 AWS 资源中修改的设置数量庞大，当尝试配置由多个 Lambda 函数、消息队列和 IAM 角色组成的无服务器系统时，CloudFormation 模板可能会变得难以管理。有各种工具在 CloudFormation 周围添加抽象层并帮助配置无服务器系统。诸如 Serverless Framework 和

Serverless Application Model (SAM) 等工具就是为了解决这个问题而创建的。

你可以将 SAM 视为 CloudFormation 顶层的层，将无服务器应用程序最相关的设置带到前面。你可以在 AWS SAM 文档 上线找到 SAM 的完整规范，但为了给你一个概述，这里是我们 C# 简历上传系统的 SAM YAML 文件的一部分。

```
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: Resume Uploader Serverless C# Application.
Resources:
  SaveUploadedResumeLambda:
    Type: AWS::Lambda::Function
    Properties:
      Handler:
ServerlessResumeUploader::ServerlessResumeUploader.
      LambdaFunctions::SaveUploadedResume
      Role: arn:aws:iam::0000000000:role/ResumeUploaderLambdaRole
      Runtime: dotnetcore3.1
      MemorySize: 256
      Timeout: 30
  LookForGithubProfileLambda:
    Type: AWS::Lambda::Function
    Properties:
      Handler:
ServerlessResumeUploader::ServerlessResumeUploader.
      LambdaFunctions::LookForGithubProfile
      Role: arn:aws:iam::0000000000:role/ResumeUploaderLambdaRole
      Runtime: dotnetcore3.1
      MemorySize: 256
      Timeout: 30
```

你可以看到我们如何定义 SaveUploadedResumeLambda 和 LookForGithubProfileLambda Lambda 函数，指明 C# 代码中入口点的位置，并为其执行配置内存、超时和权限设置。

使用这样的 SAM 文件配置你的基础设施后，你可以轻松部署用于测试或暂存的新环境。你受益于代码审查，并且可以为你的 *resources* 创建与应用程序代码类似的自动化部署管道。

## Conclusion

无服务器计算允许你构建复杂但灵活且可扩展的解决方案，采用按需付费的定价模式，并且可以缩减到零。个人而言，每当我们需要快速构建东西来验证一个想法、解决业务问题或预算有限时，我们都会使用无服务器执行模型。因为你只为所使用的付费，以 AWS Lambda 为中心的无服务器架构可以成为构建最小可行产品（MVP）或部署移动应用后端的一种极低成本的方式。AWS 提供每月高達 100 万次 Lambda 执行的免费额度，这足以让一家初创公司走出想法阶段或对产品进行 Beta 测试。本章中我们介绍的其他服务也都有非常慷慨的免费层，允许你在不破产的情况下尝试想法和架构。

话虽如此，无服务器并不会自动使你的系统运行成本更低。你需要考虑设计应用程序，避免不必要的大量 AWS Lambda 调用。仅仅因为两个函数 *可以* 是两个独立的 Lambda，并不总是意味着从成本和性能角度来看它们 *应该* 是。实验并测量。在较高负载下，函数即服务也可能很昂贵，如果你的应用程序没有架构成高效利用每次调用。 因为你按执行付费，在非常高的负载下，你可能会发现成本飙升远远超过简单地在 EC2 或 ElasticBeanstalk 上运行 `dotnet` 进程的成本。无服务器的成本应始终与可扩展性等其他优势相权衡。

无服务器架构的另一个巨大优势是灵活性，正如本章的示例所示。在每一步中，我们都通过非常小的更改彻底改变了 Serverless C# Resume Uploader 的一部分架构——大多数时候甚至无需重新部署应用程序的其余部分。这种系统 moving parts 之间的分离使增长变得更加容易，并解锁了开发团队内部人才的多样化。无服务器架构中没有任何内容规定各个组件使用什么版本、技术，甚至编程语言。你是否曾经想尝试 F#？在构建在 AWS Lambda 上的系统上，没有任何东西阻止你用 F# Lambda 编写最新功能并将其插入到你的无服务器架构中。需要将一些 HTTP 调用路由到第三方 API？如果需要，你可以直接从 API Gateway 代理它，而无需在代码中创建接口和“管道”。通过采用基础设施即代码工具，如 SAM（或第三方框架，如 Serverless Framework 和 Terraform），你可以自动化对基础设施的更改，并在基础设施配置本身上运行代码审查、拉取请求和自动化测试管道。对无服务器系统的拉取请求通常由两部分组成：一个简单、易于审查的 AWS Lambda，以及 SAM/Terraform/Cloudformation 模板中的一个条目，显示该 Lambda 如何与系统的其余部分集成。

1 预签名 URL 是指向 S3 资源的链接，可以从任何地方用于匿名下载其内容，即在浏览器窗口中输入它。URL 使用发出此请求的 IAM 角色的身份验证权限进行预签名。这有效地允许你与任何与此预签名 URL 共享的人共享对受保护 S3 资源的访问权限。

2 S3 存储桶名称在某个区域的所有 AWS 账户中都是唯一的。因此，你可能发现想要的存储桶名称不可用，因为它已被另一个 AWS 账户使用。你可以通过在存储桶名称前加上产品或组织名称来避免这种情况。

3 CloudWatch 是 AWS 的一项服务，为你的整个云基础设施提供日志记录和监控。AWS Lambda 连接到 CloudWatch 并发布日志消息，你可以从 CloudWatch 控制台查看，或集成到下游的另一个 AWS 服务中。

4 此方法使用在 .NET 6 中引入的 Minimal APIs 风格来配置 ASP .NET 应用程序。Amazon.Lambda.AspNetCoreServer 也支持基于早期 .NET Core 版本构建的 .NET 应用程序，但配置略有不同，并且涉及实现 Amazon.Lambda.AspNetCoreServer.APIGatewayProxyFunction。更多信息可在 Amazon.Lambda.AspNetCoreServer 找到

5 在 C# 中使用 AWS Lambda 函数进行依赖注入是可能的，可以手工编码设置或使用第三方库。然而，你可能会经常发现

