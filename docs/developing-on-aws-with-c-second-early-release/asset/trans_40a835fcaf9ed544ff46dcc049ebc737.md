

# StateMachineExecution0001

编辑状态机

新建执行

停止执行

详细信息

执行输入

执行输出

定义

执行状态

成功

开始时间

Feb 18, 2022 06:01:00.070 PM

结束时间

Feb 18, 2022 06:01:00.410 PM

## 执行事件历史

| ID | 类型 | 步骤 | 资源 | 经过时间 (ms) | 时间戳 |
|---|---|---|---|---|---|
| 1 | ExecutionStarted |  | - | 0 | Feb 18, 2022 06:01:00.070 PM |
| 2 | TaskStateEntered | UploadResume | - | 71 | Feb 18, 2022 06:01:00.141 PM |
| 3 | LambdaFunctionScheduled | UploadResume | Lambda | CloudWatch Logs | 71 | Feb 18, 2022 06:01:00.141 PM |
| 4 | LambdaFunctionStarted | UploadResume | Lambda | CloudWatch Logs | 119 | Feb 18, 2022 06:01:00.189 PM |
| 5 | LambdaFunctionSucceeded | UploadResume | Lambda | CloudWatch Logs | 278 | Feb 18, 2022 06:01:00.348 PM |
| 6 | TaskStateExited | UploadResume | - | 278 | Feb 18, 2022 06:01:00.348 PM |
| 7 | TaskStateEntered | QueueEmail | - | 289 | Feb 18, 2022 06:01:00.359 PM |
| 8 | TaskScheduled | QueueEmail | - | 289 | Feb 18, 2022 06:01:00.359 PM |
| 9 | TaskStarted | QueueEmail | - | 300 | Feb 18, 2022 06:01:00.370 PM |
| 10 | TaskSucceeded | QueueEmail | - | 340 | Feb 18, 2022 06:01:00.410 PM |
| 11 | TaskStateExited | QueueEmail | - | 340 | Feb 18, 2022 06:01:00.410 PM |
| 12 | ExecutionSucceeded |  | - | 340 | Feb 18, 2022 06:01:00.410 PM |

图 3-7. 管理控制台中的 Step Functions 执行日志

# C# 简历上传示例：Step Functions

为了演示我们使用 AWS Step Functions 能做什么，让我们取出上一部分简历上传示例中创建的 AWS Lambda 函数，并将其从一个拆分为多个 Lambda 函数。UploadNewResume Lambda 函数可以仅负责将文件上传到 S3，然后我们再有一个第二个 Lambda 来发送电子邮件（EmailRecruitment）。HTTP 部分也可以完全抽象出来并由 API Gateway 处理。这给了我们在这两个函数开发、测试并部署之后，仍然能够在后端更灵活地更改或优化工作流

```
[assembly:
LambdaSerializer(typeof(Amazon.Lambda.Serialization.SystemTextJson.
    DefaultLambdaJsonSerializer))]

namespace ServerlessResumeUploader
{
    public class LambdaFunctions
    {
        private readonly IStorageService _storageService = new AwsS3StorageService();
        private readonly IEmailService _emailService = new AwsSesEmailService();

        ①
        public async Task<StepFunctionsState>
UploadNewResume(StepFunctionsState state,

ILambdaContext context)
        {
            byte[] bytes =
Convert.FromBase64String(state.FileBase64);
            using var memoryStream = new MemoryStream(bytes);

            state.StoredFileUrl = await
_storageService.Upload(memoryStream);

            state.FileBase64 = null;
```

```
return state;
}
public async Task<StepFunctionsState>
EmailRecruitment(StepFunctionsState state,
ILambdaContext context)
{
    await _emailService.Send("recruitment@example.com",
        $"Somebody uploaded a resume! Read it here:
{state.StoredFileUrl}\n\n" +
        $"...and check out their Github profile:
{state.GithubProfileUrl}");

    return state;
}
}
```

第一个 Lambda 函数接收文件并保存到 S3。
第二个 Lambda 向我们的招聘团队发送文件链接的电子邮件。

你可以看到这段代码与本章前面 ApplicationController 代码非常相似，只是被重构为 LambdaFunctions 类上的两个方法。
这将作为两个独立的 AWS Lambda 函数部署，使用相同的二进制文件，这是一种部署共享代码的多个 Lambda 函数的常见方式。实际的上传和发送电子邮件仍然由 AwsS3StorageService 和 AwsSesEmailService 执行，不过我们不再使用依赖注入⁵。另请注意，对于这两个函数，第一个参数现在是 state，并且两者都是 StepFunctionsState 类型。AWS Step Functions 通过在每个任务之间传递状态对象来执行你的工作流（在此情况下任务即函数）。函数会向状态添加或删除属性。我们上面函数的状态看起来像这样：

```
public class StepFunctionsState
{
    public string FileBase64 { get; set; }

    public string StoredFileUrl { get; set; }
}
```

当此状态输入传递给第一个函数 UploadNewResume() 时，它将具有由 API Gateway 从我们前端的 HTTP POST 请求设置的 state.FileBase64。函数会将此文件保存到 S3，然后在传递状态对象到下一个函数之前设置 state.StoredFileUrl。它还会从状态对象中清除 state.FileBase64。状态对象在 AWS Step Functions 中在每个步骤之间传递，由于此 base 64 字符串会相当大，我们在读取后将其设为 null 以减小传递的状态对象的大小。

我们可以像之前一样将这两个函数部署到 AWS Lambda，不过这次我们需要为每个函数指定 --function-handler 参数。函数处理程序是我们代码中充当 Lambda 入口点的 C# 函数，如“使用 AWS Lambda 和 C# 开发”中所述。

```
dotnet lambda deploy-function UploadNewResume
  --function-handler
  ServerlessResumeUploader::ServerlessResumeUploader.
    LambdaFunctions::UploadNewResume
```

```
dotnet lambda deploy-function EmailRecruitment
  --function-handler
  ServerlessResumeUploader::ServerlessResumeUploader.
    LambdaFunctions::EmailRecruitment
```

现在如果我们进入 AWS Step Functions，我们可以创建一个新的状态机，将这两个 Lambda 连接起来并连接到 API Gateway。AWS 管理控制台确实为我们提供了图形用户界面来构建这些工作流，不过，我们是工程师，所以我们将以 Amazon States Language 在 JSON 文件中编写它。这也具有至关重要的好处，即利用可以签入源代码控制的纯文本 JSON 文件，为我们提供了一个很好的基础设施即代码（IaC）部署模型。这里我们简单工作流的 JSON 配置如下所示：

```
{
  "Comment": "Resume Uploader State Machine",
```

```
"StartAt": "SaveUploadedResume",
"States": {
    "SaveUploadedResume": {
        "Type": "Task",
        "Resource": "arn:aws:lambda:eu-west-2:00000000:function:UploadNewResume",
        "Next": "EmailRecruitment"
    },
    "EmailRecruitment": {
        "Type": "Task",
        "Resource": "arn:aws:lambda:eu-west-2:00000000:function:EmailRecruitment",
        "End": true
    }
}
```

Lambda 函数的 ARN（Amazon 资源名称）可以在管理控制台的 AWS Lambda 部分找到，它允许我们从 AWS 中的任何地方引用这些 Lambda 函数。如果我们将上面的 JSON 复制到 AWS Step Functions 状态机的定义中，你可以在控制台中看到它为我们创建了工作流的图形表示。本章稍后的图 3-11 显示了它看起来是什么样子。

此示例的最后一步是将我们状态机的开始连接到 API Gateway。以前 API Gateway 配置为将所有请求代理到我们的 AWS Lambda 函数，现在我们希望在 API Gateway 中指定确切的路由并将其转发到我们的 Step Functions 工作流。图 3-8 显示了名为 /applications 的 POST 端点的设置，其 AWS 服务设置为“Step Functions”。你也可以在图 3-8 中看到我们已将内容处理设置为“Convert to text (if needed)”。这是专门针对这里简历上传示例的一个选项。由于我们将以原始二进制形式向 API POST PDF 文件，此选项告诉 API Gateway 将其转换为 Base64 文本供我们使用。然后它可以轻松地添加到我们的状态对象（前面显示的 StepFunctionsState.cs 所表示）并在 AWS Step Functions 工作流的 Lambda 函数之间传递。

# /application - POST - 设置

选择新方法的集成点。

集成类型
Lambda Function
HTTP
Mock
AWS Service
VPC Link

AWS 区域
eu-west-2

AWS 服务
Step Functions

AWS 子域

HTTP 方法
POST

操作类型
使用操作名称
使用路径覆盖

操作
StartExecution

执行角色
arn:aws:iam::XXXXXXXXXX:role/MyLambdaRole

内容处理
Convert to text (if needed)

使用默认超时

保存

图 3-8. 用于触发 Step Functions 的 API Gateway 配置

最后，我们可以在 API Gateway 中设置一些请求映射，以将请求转换为告诉 AWS Step Functions 哪个状态

