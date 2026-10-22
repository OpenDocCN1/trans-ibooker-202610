

+   a. 服务账号名称
+   b. 服务账号 ID
+   c. 服务账号描述

# 服务账号详情

服务账号名称
gitlab-cicd
此服务账号的显示名称

服务账号 ID
gitlab-cicd-681 @ruicosta-blog.iam.gserviceaccount.com

服务账号描述
用于 GitLab CI/CD 的服务账号
描述此服务账号将执行的操作

CREATE   CANCEL

5. 点击 Create。

6. 为服务账号分配以下角色

+   a. Cloud Functions Developer
+   b. Service Account User

Role
Cloud Functions Developer

对所有函数相关资源具有读写权限。

Condition
Add condition

Role
Service Account User

以服务账号身份运行操作。

Condition
Add condition

+   7. 点击 Continue
+   8. 点击 Done
+   9. 找到新创建的服务账号，点击操作图标，然后选择 create a key：
+      :
+      :
+   10. 选择 JSON 并点击 CREATE。这将下载 JSON 文件到你的本地工作站。
+   11. 前往你的 GitLab 项目，并在设置中进入 CI/CD
+   12. 点击 Expand Variables
+   13. 创建两个新的变量，标签为：
+       a. PROJECT_ID
+       b. SERVICE_ACCOUNT
+   14. 输入你的 Google Cloud 项目 ID
+   15. 打开之前下载的服务账号 JSON 文件，复制其内容粘贴到 SERVICE_ACCOUNT 键中。它应类似于图 3-18：

Figure 3-18. GitLab Variables

此时你已为 GitLab 配置了授权，以便将其部署到你的 Google Cloud 项目中的 Cloud Functions。下一步是为音频文件上传和 Pub/Sub 主题创建存储桶。

+   1. 要在不进入 Cloud Console 的情况下快速创建存储桶，你可以运行以下命令：`gsutil mb gs://[BUCKET_NAME]` / 请注意，存储桶名称需要在 Google Cloud 客户之间是唯一的，因为它是一个全球可访问的资源。此外，存储桶名称只能包含小写字母、数字、连字符、下划线和点。不允许使用空格。
+   2. 创建一个新的 GitLab 变量，如步骤 13 中所做，名为 TRIGGER_RESOURCE
+   3. 输入新创建的存储桶名称作为 GitLab 变量的值，不要在值中包含 `gs://` （图 3-19）

Figure 3-19. GitLab Variables

+   4. 此存储桶将保存音频文件，并将成为触发你的 Cloud Function 的资源。
+   5. 运行以下命令创建 Pub/Sub 主题：`gcloud pubsub topics create my-topic`

![](img/eac8762c24ec77948aa0c3277f8e6eb3_106_0.png)

![](img/eac8762c24ec77948aa0c3277f8e6eb3_106_1.png)

a. 当你向 Pub/Sub 发布消息时，Pub/Sub 会将消息存储在最近的 Google Cloud 区域。Pub/Sub 然后将消息传递给全球各地的订阅者。

```
If you need to restrict the regions you can run the following command which restricts the region to us-central1: gcloud pubsub topics update [YOUR_TOPIC] --message-storage-policy-allowed-regions=us-central1
```

下一步是准备你的应用程序代码以推送到 GitLab 仓库的主分支。

+   1. Git clone 以下仓库：to come
+   2. 将克隆仓库中的 saf-longrun-job-func 文件夹复制到你的 GitLab 仓库
+   3. 提交更改并将代码推送到你的仓库
+   4. 如果你返回到 GitLab CI/CD，你应该会看到你的作业正在运行：
+   5. 管道成功执行后，你应该会看到以下消息：
+   6. 在 Google Cloud Console 中打开 Cloud Function：
+   7. 你应该会看到 Cloud Function 已成功部署。
+   8. 点击你部署的函数名称，然后点击 Trigger 选项卡，这将列出触发器类型的配置详情以及在 GitLab yml 文件中定义的关联资源，如：
+   9. 在上传音频文件之前，你需要启用 Cloud Speech-to-Text API。云函数将把音频文件发送到 Speech-to-Text API 并

![](img/eac8762c24ec77948aa0c3277f8e6eb3_107_0.png)

![](img/eac8762c24ec77948aa0c3277f8e6eb3_107_1.png)

![](img/eac8762c24ec77948aa0c3277f8e6eb3_107_2.png)

```
gcloud --quiet --project $PROJECT_ID functions deploy safLongRunJobFunc --runtime=nodejs8 --trigger-event=google.storage.object.finalize --trigger-resource=$TRIGGER_RESOURCE
```

它将接收一个作业名称，该名称将包含在发送到 Pub/Sub 的消息中。你可以通过运行以下命令启用 API：

```
gcloud services enable speech
```

为了在上下文中展示，你刚刚配置并部署了图 3-20 中圈出的组件

![](img/eac8762c24ec77948aa0c3277f8e6eb3_108_0.png)

Figure 3-20. Framework Architecture

对于下一部分，你将测试刚刚部署的组件。在你克隆的仓库中，你将有一个名为 sample audio files 的文件夹，你将使用这些音频文件来测试已部署的组件。

+   1. 在终端中运行以下命令处理示例立体声音频文件，用方括号 [] 中的参数替换为你的值。示例音频文件位于克隆仓库的以下文件夹中：/chapter-three/sample-audio

```
gsutil -h x-goog-meta-callid:1234567 -h x-goog-meta-stereo:true -h x-goog-meta-pubsubtopicname:[TOPIC_NAME] -h x-goog-meta-year:[YEAR] -h x-goog-meta-month:[MONTH_AS_04] -h x-goog-meta-day:[DAY_AS_06] -h x-goog-meta-starttime:[START_TIME_AS_0910] cp stereo.wav gs://[YOUR_UPLOADED_AUDIO_FILES_BUCKET_NAME]
```

2. 在终端中运行以下命令处理示例单声道音频文件，用方括号 [] 中的参数替换为你的值。

```
gsutil -h x-goog-meta-callid:1234567 -h x-goog-meta-stereo:false -h x-goog-meta-pubsubtopicname:[TOPIC_NAME] -h x-goog-meta-year:[YEAR] -h x-goog-meta-month:[MONTH_AS_04] -h x-goog-meta-day:[DAY_AS_06] -h x-goog-meta-starttime:[START_TIME_AS_0910] cp mono.flac gs://[YOUR_UPLOADED_AUDIO_FILES_BUCKET_NAME]
```

3. 你不会在 Cloud Pub/Sub 中看到消息，你可以通过查看日志以及检查订阅来查看是否成功，你应该会看到图 3-21 所示的输出：

Message 1396373909145028 published.

![](img/eac8762c24ec77948aa0c3277f8e6eb3_109_0.png)

Figure 3-21. Screenshot of Cloud/Pub Message Body and Body Keys

你已成功部署并测试了云原生应用程序的第一部分。

# Cloud Native Checkpoint

让我们再次回顾我们为云原生架构列出的考虑因素，以及我们如何通过 Cloud Pub/Sub 解决每个问题。

+   1. 我们的应用程序需要从云基础设施中抽象出来。在这里你使用 Cloud Pub/Sub，它使你免于管理队列系统，无需担心可扩展性，并让你专注于应用程序功能。✓

+   2. 我们需要一种持续集成和持续交付的方法。这对于 Cloud Pub/Sub 不适用，因为管理消息的代码将由 Cloud Functions 和 Cloud Dataflow 处理。✓
+   3. 我们的应用程序必须能够根据需要扩展和缩减。“Google 产品包括 Ads、Search 和 Gmail 使用此基础设施每秒发送超过 5 亿条消息，总计超过 1TB/s 的数据” - https://cloud.google.com/pubsub/architecture ✓
+   4. 我们的应用程序将如何管理故障？管理故障的关键是主动监控应用程序和 Cloud Pub/Sub。在本书第 X 章的监控部分，你学习了如何为未确认的消息启用警报。这将是提供可靠服务的一个关键组件，因为它能让你知道 Cloud Pub/Sub 何时不健康。✓
+   5. 应用程序需要全局可访问。单条消息存储在单个区域，但主题可以有存储在多个区域的消息。✓
+   6. 我们的框架需要基于微服务架构。尚不适用，但一旦你将应用程序部署到 Kubernetes 后就会适用。
+   7. 我们的应用程序将如何得到保护？在本章中你学习了如何使用个人凭据使用 Cloud Pub/Sub 库，你还使用 GitLab 和受限服务账号将应用程序部署到 Cloud Functions。✓

# Closing Remarks

在本章中你学习了 Cloud Pub/Sub 以及它如何应用于云原生应用程序。Cloud Pub/Sub 是一个强大的服务，允许应用程序在彼此之间传递消息而不会引入延迟。它根据需要进行扩展，并且由于它是托管服务，它让你专注于应用程序代码。你还部署了应用程序的第一部分，并且随着章节的进展你将继续在此基础上构建。在下一章，你将学习 Cloud Dataflow。这将是本章中你创建的 Cloud Pub/Sub 主题的订阅者，以检索由 Cloud Functions 发送的消息。Cloud Pub/Sub 是 Cloud Functions 和 Cloud Dataflow 之间的桥梁，而 Cloud Dataflow 将是我们丰富数据的过程。

