

```csharp
public class AwsS3StorageService : IStorageService
{
    const string BucketName = "csharp-examples-bucket";

    public async Task<string> Upload(Stream stream)
    {
        var fileName = Guid.NewGuid().ToString() + ".pdf"; ①

        using var s3Client = new AmazonS3Client(RegionEndpoint.EUWest2);

        await s3Client.PutObjectAsync(new PutObjectRequest()
        {
            InputStream = stream,
            BucketName = BucketName,
            Key = fileName,
        }); ②

        var url = s3Client.GetPreSignedURL(new GetPreSignedUrlRequest()
        {
            BucketName = BucketName,
            Key = fileName,
            Expires = DateTime.UtcNow.AddMinutes(10)
        }); ③

        return url;
    }
}
```
① 创建一个唯一 S3 键名。
② 将文件上传到 S3
③ 生成指向我们新文件的预签名 URL。¹

这使用了 AWSSDK.S3 NuGet 包将文件保存到 AWS S3 存储桶中。在此示例中，存储桶名为“csharp-examples-bucket”²，并且文件将被上传并使用 Guid.NewGuid() 赋予唯一键。

我们示例中的第二个服务是 IEmailService，它使用 AWS Simple Email Service (SES) 向招聘团队发送电子邮件。此实现使用了 AWS SDK 中的另一个 NuGet 包，名为 Amazon.SimpleEmail。

```csharp
public class AwsSesEmailService : IEmailService
{
    public async Task Send(string emailAddress, string body)
    {
        using var emailClient = new AmazonSimpleEmailServiceClient(
            RegionEndpoint.EUWest1);

        await emailClient.SendEmailAsync(new SendEmailRequest
        {
            Source = "from@example.com",
            Destination = new Destination
            {
                ToAddresses = new List<string> { emailAddress }
            },
            Message = new Message
            {
                Subject = new Content("Email Subject"),
                Body = new Body { Text = new Content(body) }
            }
        });
    }
}
```

我们将在本章其余示例中使用这两个实现。如前所述，通过使用 S3 保存 PDF 文件并通过 SES 发送电子邮件，我们已经在使用无服务器解决方案进行文件存储和电子邮件服务。那么让我们也去掉这个 Web 服务器，并将我们的控制器操作迁移到托管的云函数中。

## 使用 AWS Lambda 和 C# 开发

AWS 的 FaaS 产品称为 Lambda。AWS Lambda 由 Amazon 首席技术官 Werner Vogels 于 2014 年推出，我们认为以下总结很好地概括了 AWS Lambda 的动机和价值。

> 这里的重点是事件。事件可能由会触发这些事件的 Web 服务驱动。你将编写一些代码，比如用 JavaScript，然后它会在无需为其预配硬件的情况下运行。
> 
> ——Werner Vogels

自 2014 年以来，AWS Lambda 在采用率和功能方面都获得了巨大的增长。他们为触发函数添加了越来越多的事件选项，提供了像 Lambda@Edge 这样前沿的能力，该能力在离用户最近的 CDN 服务器上运行你的函数（即“边缘计算”），自定义运行时、用于库的共享内存层称为“Lambda 层”，以及对我们至关重要的，在 x86_64 和 ARM64 CPU 架构上都内置支持 .NET。

AWS 提供了大量关于用 C# 编写 Lambda 函数以及重构现有代码以便部署到 AWS Lambda 的示例。以下是利用 Amazon.Lambda.Templates nuget 包中的模板快速创建并部署 C# 简单 Lambda 函数的示例。该包包含可与 .NET Core CLI 上的 dotnet new 命令一起使用的项目模板。

```
dotnet new -i Amazon.Lambda.Templates

dotnet new lambda.EmptyFunction --name SingleCSharpLambda
```

这将创建一个名为“SingleCSharpLambda”的文件夹，其中包含函数的源项目和测试项目。示例函数位于名为 Function.cs 的文件中。

```csharp
[assembly: LambdaSerializer(typeof(Amazon.Lambda.Serialization.SystemTextJson.DefaultLambdaJsonSerializer))]

namespace SingleCSharpLambda
{
    public class Function
    {
        public string FunctionHandler(string input, ILambdaContext context)
        {
            LambdaLogger.Log("Hello from" + context.FunctionName);

            return input?.ToUpper();
        }
    }
}
```

模板创建了一个带有公共函数 `FunctionHandler(input, context)` 的类。这是 AWS Lambda 中每个 C# Lambda 函数入口点的 方法签名。两个参数是输入，其形状将根据我们将 Lambda 函数挂钩到的事件而定，以及 `ILambdaContext context`，该上下文由 AWS 在执行时生成，并包含有关当前正在执行的 Lambda 函数的信息。

我们还在上面的模板函数中添加了一行用于打印日志消息，以便我们可以在 AWS 上运行时检查其是否执行。这里的静态类 `LambdaLogger` 是 `Amazon.Lambda.Core` 包的一部分，该包随模板一起添加。你也可以在此处使用 `Console.WriteLine()`，AWS 会将写入 `stdout` 或 `stderr` 的任何调用发送到附加到你的函数的 CloudWatch³ 日志流。

现在我们可以将函数部署到 AWS。如果尚未安装，现在是获取 AWS Lambda .NET 全局工具的好时机。全局工具是一种特殊的 NuGet 包，你可以从 `dotnet` 命令行执行它。

```
dotnet tool install -g Amazon.Lambda.Tools
```

然后部署你的函数。

```
dotnet lambda deploy-function SingleCSharpLambda
```

如果 `aws-lambda-tools-defaults.json` 配置文件中尚未设置，你将被要求提供 AWS 区域以及你希望函数在执行时使用的 IAM 角色的 ARN。IAM 角色可以由 `Amazon.Lambda.Tools` 工具即时创建，其中包含所需的所有权限。你可以通过访问 IAM 控制台来查看和编辑此（或任何其他）IAM 角色的权限。

图 3-2 显示了我们在部署到的区域的 AWS 管理控制台中的示例函数。你可以从下一屏幕的“Test”选项卡直接在控制台中测试该函数。

![](img/044e6b5616cc4c02f2683d0cc9990365_91_0.png)

图 3-2. AWS Lambda 控制台中的函数列表

此测试窗口（如图 3-3 所示）允许你在输入参数中输入一些 JSON 以传递给函数，该参数我们在前面定义过。由于我们将输入值声明为 `FunctionHandler(string input, ILambdaContext context)` 中的 string 类型

```
FunctionHandler(string input, ILambdaContext context)
```

我们应该将其更改为任意字符串，我们就可以直接在管理控制台中测试该函数。

Lambda > Functions > SingleCSharpLambda

# SingleCSharpLambda

Function overview Info

Code Test Monitor Configuration Aliases Versions

Execution result: succeeded (logs)

Details

The area below shows the last 4 KB of the execution log.

```
"TEST"
```

# Log output

The section below shows the logging calls in your code. Click here to view the corresponding CloudWatch log group.

```
START RequestId: 962f432c-d59a-4be2-be22-833dad082c3e Version: $LATEST
CONTEXT:
{"FunctionName":"SingleCSharpLambda","FunctionVersion":"$LATEST","LogGroupName":"/aws/lambda/SingleCSharpLam
```

# Test event

Invoke your function with a test event. Choose a template that matches the service that triggers your function, or enter your event document in JSON.

New event

Saved event

Template

hello-world

Name

MyEventName

```
"test"
```

图 3-3. 测试已部署的 Lambda 函数

你的执行结果会被插入到页面中，其中包括一个内联的“Log output”窗口，如图 3-3 所示。这使你能够快速查找和调试 Lambda 函数中的错误并进行手动测试。

## C# 简历示例：AWS Lambda

现在我们熟悉了 AWS Lambda，让我们将其应用到我们的无服务器示例中。我们将把 C# 简历上传器示例中的 Web API 控制器变成一个 AWS Lambda 函数。任何 Web API 控制器都可以使用 AWS 的一个名为 Amazon.Lambda.AspNetCoreServer.Hosting 的包部署到 AWS Lambda。该包使用 makes 使包装 .NET 控制器在 API Gateway 调用的 Lambda 函数中变得容易

> NOTE
> 
> API Gateway 是 AWS 的一项服务，使构建由 Lambda 函数组成的 API 成为可能。它提供了 HTTP 请求与 Lambda 函数之间的管道，使你能够配置一组单独的 Lambda 函数，以便在 API Gateway 中配置的 API 路由的 GET、PUT、POST、PATCH 和 DELETE 请求上运行。我们在此示例中使用 API Gateway 将路由 POST:https://our-api/Application 映射到 Lambda 函数。

要使用此方法将 ASP .NET 应用程序部署到 AWS Lambda，请安装 Amazon.Lambda.AspNetCoreServer 包，然后在应用程序的 services 集合中添加对 AddAWSLambdaHosting() 的调用⁴。这允许 API Gateway 和 AWS Lambda 在 Lambda 上运行时充当 Web 服务器。

```
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorPages();
```
