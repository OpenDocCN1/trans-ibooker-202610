

如果你认为它是在轮询 Speech-To-Text API，你是对的！

Cloud Dataflow 将按以下顺序执行以下任务：

- 1. 从 Pub/Sub 拉取消息
- 2. 轮询 Speech-To-Text 以确定作业是否已完成
- 3. 将完成的转录发送到 Cloud Natural Language
- 4. 接收来自 Cloud Natural Language 的响应
- 5. 使用自定义定义的函数丰富数据
- 6. 将数据值匹配到 BigQuery 模式
- 7. 最后将数据写入 BigQuery

一旦 Cloud Dataflow 成功完成其管道，你的数据将在 BigQuery 中可用。图 1-6 说明了数据在 BigQuery 中将如何表示。

![](img/eac8762c24ec77948aa0c3277f8e6eb3_21_0.png)

图 1-6. BigQuery 嵌套重复字段预览

在图 1-6 中你会注意到行嵌套在行内；我们将在 BigQuery 章节中详细介绍这一点。

## 丰富数据的可视化

如所述的 Cloud Dataflow 管道的一部分是利用 Speech-To-Text 和 Natural Language API 来丰富数据。丰富数据是指提供我们用例所需要的额外数据点的过程。例如，确定通话沉默时间。我们可以使用 Cloud Dataflow 运行自定义算法来为每个音频文件制定通话沉默时间。

### 确定说话人、说话时间和沉默时间

以下代码片段是处理长音频文件的一个示例。它采用了说话人分离，是一种检测说话人何时发生变化并为音频中检测到的各个声音添加编号标签的功能。

你可以使用说话人分离和词时间戳功能来确定说话人、说话人说话时间和通话沉默。你还可以创建情感热力图以获取更多详细信息。

```
const audioConfig = {
    encoding:"FLAC",
    sampleRateHertz: 44100,
    languageCode: `en-US`,
    enableSpeakerDiarization: true,
    diarizationSpeakerCount: 2,
    enableAutomaticPunctuation: true,
    enableWordTimeOffsets: false,
    useEnhanced: true,
    model: 'phone_call'
};
const audioPath = {
    uri: `gs://${object.bucket}/${object.name}`
};
const audioRequest = {
    audio: audioPath,
    config: audioConfig,
};
return spclient
    .longRunningRecognize(audioRequest)
    .then(data => {
        const operation = data[0];
```

```
return operation.promise();
})
```

呼叫中心主管可以看到通话的进展，包括通话如何开始和结束。除了视觉进展之外，他们还可以深入每个方块以查看句子情感。

## 情感时间线

句子：I bought her a pair from a different store.

1:29 / 2:12

图 1-7. 我们使用 React.js 构建的前端框架截图

## 从对话中提取情感

我们将使用 Natural Language API 提取整体转录情感、句子情感和实体。有了这些数据用于转录的音频文件，你可以创建热力图和情感时间线。你还可以构建词云。

以下示例显示了捕获句子情感的代码片段：

```
client
    .analyzeSentiment({document: document})
    .then(results => {
        const sentences = results[0].sentences;
        sentences.forEach(sentence => {
            pubSubObj.sentences.push({
                'sentence': sentence.text.content,
                'score': sentence.sentiment.score,
                'magnitude': sentence.sentiment.magnitude
            })
        })
    });
```

图 1-7 是我们将为用例构建的示例，我们为业务用户提供句子热力图的访问权限，让他们可以深入每个音频文件并了解通话的进展。

对于用户可视化和 API，应用程序将部署在 Google Kubernetes Engine 上（我们稍后会详细介绍）。本书的 GitHub 仓库包含一个使用 Express.js 构建的 API，它利用 BigQuery Node.js SDK 运行 SQL 语句以检索数据。SQL 命令是在用户点击可视化时触发的。此 Express.js 应用程序将与用户界面一起部署在 Google Kubernetes Engine 上。

请记住，我们的事实来源将是 BigQuery。我们将通过 Web 浏览器将此数据可视化给用户，并且我们的可视化应用程序将使用 React Framework 和 Node.js 开发。

以下示例查询查找转录中所有的单词，这些单词当前存储为嵌套重复字段。查询语句使用 BigQuery SDK 执行，它获取相关记录中的所有单词。这是我们 Express.js API 的一个示例，此响应由 Web 应用程序接收。

```
const sqlQueryCallLogsWords = `SELECT
  ARRAY(SELECT AS STRUCT word, startSecs, endSecs FROM
UNNEST(words)) words
  FROM \'\' + bigqueryDatasetTable + \'\'
  where fileid = \'\' + queryFileId + \'\'`
```

## 保护应用程序

我们涵盖了大量信息，请再坚持一会儿。

对于框架的最后一个组件，我们需要实现授权以允许我们认为合适的人访问应用程序。牢记授权只是安全的一个组件；我们还必须考虑加密和保护敏感数据等事项。现在，让我们专注于身份验证。Google Cloud 提供了一项名为 Identity-Aware Proxy（IAP）的服务。IAP 通过验证用户的身份和请求的上下文来允许或拒绝对应用程序的访问。我喜欢把它想象成由 Google Cloud 管理的代理，位于我们的应用程序前面，拦截请求并代表我们授权用户。

为什么 IAP 是革命性的？IAP 使开发人员无需构建后端身份验证系统，从而使开发人员能够专注于应用程序的核心功能。基本上 IAP 允许我们更快地进入市场。我们的应用程序将没有任何授权代码，对的，没有用于授权用户的代码。意味着没有来自第三方用于验证令牌的 SDK 或获取和验证 JSON Web Token 的任何其他复杂代码。我们所有的

应用程序将做的只是专注于其核心角色，即为我们转录和丰富过的音频文件向用户提供洞察。

IAP 还支持多种提供商类型：

- Email and password
- OAuth（例如 Google、Facebook、Twitter 等）
- SAML
- OIDC
- Phone number
- Custom
- Anonymous

我们可以在更快进入市场的同时保护应用程序，提供广泛的身份验证提供商，并通过不需要 VPN 客户端来访问应用程序来简化用户访问。

## Cloud Native Checkpoint

现在你对用例和我们将用于满足用例要求的框架有了全面的了解，让我们重新审视我们为云原生架构列出的考虑因素：

- 1. 我们的应用程序需要从云基础设施中抽象出来。
- 2. 我们需要一种持续集成和持续交付的方法。
- 3. 我们的应用程序必须能够根据需要扩展和缩减。
- 4. 我们的应用程序将如何管理故障？
- 5. 应用程序需要全球可访问。
- 6. 我们的框架需要基于微服务架构。
- 7. 我们的应用程序将如何得到保护？

## 从云基础设施中抽象出来

从云基础设施中抽象出来可能是一个具有误导性的术语。当我第一次开始学习云原生框架时，我总是认为这是一种云无关的方法：构建一个应用程序并部署到任何云提供商。然而，从云基础设施中抽象出来是应用程序与底层基础设施的解耦。但如果解耦了，我们是否不能在任何云提供商甚至本地运行它？这就是云原生可以被个人不同解读的地方。我们构建云原生应用程序是为了按需配置资源、自动化应用程序生命周期、以及可扩展和安全。然而，始终存在对云提供商及其构建云原生应用程序的核心要求的依赖。我们在构建云原生应用程序时需要考虑这些，我们将在本书中贯穿始终。但请记住，本书是关于在 Google Cloud 上运行云原生应用程序的，对于这一点，我们将尽可能专注于使用其服务构建可扩展和安全的应用程序。

## 持续集成和持续交付

云原生应用程序的核心组件之一是持续集成（CI）和持续交付（CD）架构。在我们的框架中，我们将使用 GitLab CI/CD 服务来创建我们的部署管道。我们已为此服务跳出了 Google Cloud，但我们仍然在云原生应用程序框架内。我们仍然满足拥有管道来构建、测试和验证新代码的核心要求，然后在将更改合并到你的 GitLab 仓库之前，以及确保我们的

