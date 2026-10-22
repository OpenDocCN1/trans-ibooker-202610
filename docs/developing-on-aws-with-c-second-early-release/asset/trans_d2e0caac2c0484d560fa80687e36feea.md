

这部分不涉及任何服务器的配置或管理，因此是 *serverless* 的。

你很可能已经在某些任务中使用了无服务器计算。考虑三种流行的服务器类型：

- Web 服务器
- 电子邮件服务器
- 文件服务器

在这三种中，后两种是我们很早就开始用托管的无服务器解决方案替换的服务器。如果你曾经通过 API 调用向 Mailchimp、Mailgun、SendGrid、SparkPost，或确实是亚马逊的简单电子邮件服务（SES）发送过电子邮件，那么你就已经使用过无服务器的电子邮件解决方案了。对于许多组织来说，自行或在云中运行 SMTP 服务器的日子早已牢牢地留在过去了。文件服务器也正在逐渐过时，被无服务器文件存储解决方案所取代。许多现代 Web 应用程序完全依赖于云原生服务，例如亚马逊简单存储服务（S3）作为其文件的主要存储机制。上面列表中最后要倒下的那块石头是 Web 服务器。在本章中，我们将向你展示如何用无服务器的云原生实现替换你的 .NET Web 服务器，以及这对你编写代码的方式意味着什么。

## 无服务器 Web 服务器

Web 服务器是一台物理或虚拟的计算机，它永久连接到互联网，并且 24 小时随时准备响应 HTTP(S) 请求。HTTP 请求在 Web 服务器内部的生命周期涉及许多步骤，并且执行由许多不同方编写的代码以读取、转换和执行每个请求。就在这段旅程的正中间，是由你、应用程序开发者编写的代码。

为了复制 Web 服务器的功能，我们需要一种按托管且按需的方式运行该自定义逻辑的方法。AWS 为我们提供了几种方法，我们将查看一种服务，它允许我们在无需担心服务器的情况下部署完全托管的容器化应用程序。AWS Fargate 是另一种无服务器解决方案，用于按需付费定价模式运行 Web 服务器。这些服务将你的 Web 服务部署到云中，采用无服务器方式。然而，我们可以更进一步，将服务本身拆分为独立部署到云中的各个函数。为此，我们需要函数即服务（FaaS）。

FaaS 解决方案，例如 AWS Lambda，是由事件触发的无状态计算容器。你上传在事件发生时要运行的代码，云提供商将为你处理资源的配置、执行然后取消配置。这带来了两个主要优势：

- 1. FaaS 允许你的代码缩减到零。你只需为 *函数执行期间* 使用的资源付费，而中间的时间则无需付费。这意味着你可以编写遵循真正的按需付费定价模型的应用程序，只为每次函数执行付费，而不为两次执行之间的未使用时间付费。
- 2. FaaS 迫使你思考关注点的分离，并对你编写应用程序的方式施加限制，这通常会导致更好的代码。

> *简单是困难的工作。但回报巨大。拥有真正简单系统的人——一个由真正简单部分构成的系统——将能够以最少的工作产生最大的改变。他会击败你。他会在前期花费更多时间简化事物，从长远来看他会彻底击败你，因为他有在你挣扎着推动大象时改变事物的能力。*
> 
> —Rich Hickey，Clojure 编程语言的创建者

简单性与为 FaaS 编写代码相关意味着要针对事件，你希望保持上面第 1 点为真，以便句子“当 X 发生时执行 Y”对每次事件 X 发生时都成立。你还需要保持你的函数无状态（上面第 2 点）。这既是运行你的函数在托管 FaaS 上的限制，也是其特性。因为 AWS 可以并且会在你的函数执行之间取消配置资源，所以无法共享静态变量。如果你想在 FaaS 函数中持久化数据，你必须有意将其持久化到共享存储区，例如通过保存到数据库或 Redis 缓存。

将你的代码架构为这种无状态且无副作用的函数可能是一个需要纪律的挑战。它迫使你思考如何编写逻辑以及代码的每个区域实际被允许控制多少范围。它也迫使你思考关注点的分离。

当多个 FaaS 函数使用事件连接在一起并通过其他无服务器服务（例如消息队列和 API 网关）增强时，就有可能构建一个后端应用程序，它能够执行传统服务器上运行的传统 Web API 可以执行的任何功能。

这使你能够在 AWS 上编写完全无服务器的后端应用程序。最好的部分是，你可以用 C# 完成所有这些！AWS 提供了许多服务，你可以用它们作为此类无服务器应用程序的构建块，让我们来看看他们提供的一些组件。

## 为 .NET 在 AWS 上选择无服务器组件

我们将向你介绍 AWS 上最有用的一些无服务器服务，以及如何使用 AWS SDK for .NET 的各种包在你的代码中与这些服务交互。让我们在剩余的本章中逐步构建一个无服务器 Web 应用程序，随着每个新概念的引入而添加功能。

想象一下，我们经营一家软件开发咨询公司，正在招募最优秀、最聪明的 C# 开发者！为了让他们申请，我们希望他们访问我们的网站，在那里他们会找到一个 HTML 表单，允许他们上传简历的 PDF 文件。提交此表单会将他们的 PDF 简历发送到我们的 API，在那里它将被保存到文件服务器，并向我们的招聘团队发送一封电子邮件，请求他们审阅。 我们将称之为无服务器 C# 简历上传器。图 3-1 显示了架构的高级概述。

www.example.com

# 欢迎来到我们的网站！

在此上传你的简历

上传文件...

PDF 文件

C#

Web API

S3 存储桶

电子邮件

## 图 3-1. 无服务器 C# 简历上传器架构

我们后端当前实现使用 Web API 控制器操作来接受 PDF 文件上传，将其保存到云存储并向我们的招聘团队发送电子邮件。代码如下所示：

```
[ApiController]
[Route("[controller]")]
public class ApplicationController : ControllerBase
{
    private readonly IStorageService _storageService;
    private readonly IEmailService _emailService;

    public ApplicationController(IStorageService storageService, IEmailService emailService)
    {
        _storageService = storageService;
        _emailService = emailService;
    }

    [HttpPost]
    public async Task<IActionResult> SaveUploadedResume()
    {
        Request.EnableBuffering();
        using var fileStream = new MemoryStream();
        using var reader = new StreamReader(fileStream);
        await Request.Body.CopyToAsync(fileStream); ①

        var storedFileUrl = await
        _storageService.Upload(fileStream); ②

        await _emailService.Send("recruitment@example.com",
            $"Somebody has uploaded a resume! Read it here: {storedFileUrl}"); ③

        return Ok();
    }
}
```

① 从请求中读取上传的文件。
② 将其保存到云存储。
③ 向我们的招聘团队发送一封包含文件链接的电子邮件。

IStorageService 的代码如下所示：

