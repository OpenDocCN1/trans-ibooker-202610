

冷启动会影响应用程序的性能。Google 提供了一系列提示和技巧来帮助我们减少冷启动的影响。查看 *Further reading* 部分以获取详细指南的链接。

## 本地模拟器

部署函数到 GCP 需要时间。如果您想加快测试速度，可以使用本地模拟器。这仅适用于 Node.js，并且允许您部署、运行和调试函数。

在下一节中，我们将学习如何部署 Cloud Function。

## 部署 Cloud Functions

Cloud Functions 可以使用 CLI、Google Cloud Console 或 API 进行部署。在本节中，我们将查看前两种方法，因为在考试中您很可能会被要求测试它们。

### 使用 Google Cloud Console 部署 Cloud Functions

要从 Google Cloud Console 部署 Cloud Functions，请按照以下步骤操作：

- 1. 从汉堡菜单中选择 **Cloud Functions**。您将看到 **Cloud Functions** 窗口。点击 **CREATE FUNCTION**：

![](img/de2c97ab017f36b3750a5bade111edd7_275_0.png)

图 9.3 – Cloud Functions 菜单

2. 填写函数的名称和托管区域：

### Basics

Function name *
testFunction

Region
us-central1

### Trigger

### HTTP

Trigger type
HTTP

URL
https://us-central1-fir-73fad.cloudfunctions.net/testFunction

### Authentication

Allow unauthenticated invocations
如果您正在创建公共 API 或网站，请勾选此项。

Require authentication
使用 Cloud IAM 管理授权用户。

Require HTTPS

SAVE   CANCEL

RUNTIME, BUILD AND CONNECTIONS SETTINGS

图 9.4 – 创建函数

3. 从下拉菜单中选择触发器类型：

### Trigger

HTTP

Trigger type

- HTTP
- Cloud Pub/Sub
- Cloud Storage
- Cloud Firestore
- Google Analytics for Firebase PREVIEW
- Firebase Authentication PREVIEW
- Firebase Realtime Database PREVIEW
- Firebase Remote Config PREVIEW

SAVE

CANCEL

图 9.5 – 触发器

4. 点击 **SAVE** 按钮。如果您选择使用 **HTTP** 触发器，调用函数的 URL 将为您在 **URL** 部分生成，如下所示：

### Trigger

![](img/de2c97ab017f36b3750a5bade111edd7_278_0.png)

图 9.6 – Cloud Function URL

5. 如果您想设置更高级的设置，请点击 **RUNTIME, BUILD AND CONNECTION SETTINGS**。在 **RUNTIME** 选项卡中，您可以设置 **Memory allocated** 和 **Timeout**。您还可以定义将运行函数的服务账号。最后，您可以定义最小和最大实例数：

### RUNTIME, BUILD AND CONNECTIONS SETTINGS

RUNTIME

BUILD

CONNECTIONS

Memory allocated *
256 MiB

Timeout *
60
seconds

### Runtime service account

Runtime service account
App Engine default service account

### Autoscaling

Maximum number of instances

### Runtime environment variables

+ ADD VARIABLE

图 9.7 – 运行时选项

6. 在 BUILD 选项卡中，您可以定义用于构建函数的变量和工作池：

RUNTIME

BUILD

CONNECTIONS

SECURITY

### Build Worker Pools

Selected Environment

### Build environment variables

+ ADD VARIABLE

图 9.8 – 构建选项

7. 在 **CONNECTIONS** 选项卡中，您可以定义各种入站设置。这可以限制触发函数的流量来源。您可能希望创建一个连接器，以便函数可以连接到您的 VPC：

### Runtime, build, connections and security settings

RUNTIME   BUILD   CONNECTIONS   SECURITY

Ingress settings ?

- Allow all traffic
- Allow internal traffic only
  仅允许来自同一项目中 VPC 网络或同一 VPC SC 周边内的流量。
- Allow internal traffic and traffic from Cloud Load Balancing
  允许来自同一项目中 VPC 网络、同一 VPC SC 周边或来自 Cloud Load Balancing 的流量。

Egress settings ?

默认情况下，您的函数可以向互联网发送请求，但不能向 VPC 网络中的资源发送请求。要向 VPC 网络中的资源发送请求，请创建或选择已在与函数相同区域中创建的 VPC 连接器。

VPC Connector
None
Create a Serverless VPC Connector

- Route only requests to private IPs through the VPC connector
- Route all traffic through the VPC connector

图 9.9 – CONNECTIONS

8. 在 SECURITY 选项卡中，您可以设置可由函数使用的密钥：

### Runtime, build, connections and security settings

RUNTIME   BUILD   CONNECTIONS   SECURITY

### Secrets

### Reference a Secret

Secret *

Reference method
Mounted as volume

/ Mount path

Specified paths for secret versions

+ ADD

CANCEL   DONE

REFERENCE A SECRET

图 9.10 – SECURITY

9. 点击 Next 按钮。选择您将如何提供代码。您可以使用 Inline Editor，从本地机器上传（ZIP Upload）或从 Cloud Storage ZIP，或甚至使用 Cloud Source 仓库：

![](img/de2c97ab017f36b3750a5bade111edd7_282_0.png)

图 9.11 – 源代码

10. 从 Runtime 下拉菜单中，选择您将用于编写函数的编程语言。对于此示例，我们决定使用内联编辑器用于 Python 3.7。因此，我们需要提供两个文件：main.py，其中我们将定义函数，以及 requirements.txt，其中包含依赖项：

图 9.12 – main.py 文件

11. 在 Entry point 字段中，我们必须定义入口点的名称；例如，hello_world：

![](img/de2c97ab017f36b3750a5bade111edd7_283_0.png)

图 9.13 – 入口点

12. 现在，我们只需点击 DEPLOY 按钮即可部署函数：

![](img/de2c97ab017f36b3750a5bade111edd7_283_1.png)

图 9.14 – DEPLOY

13. 一旦您的函数已部署，您将在 Cloud Functions 列表中看到它：

![](img/de2c97ab017f36b3750a5bade111edd7_283_2.png)

图 9.15 – 函数已部署

现在，您的函数已准备好执行。

```
def hello_world(request):
    """Responds to any HTTP request.
    Args:
        request (flask.Request): HTTP request object.
    Returns:
        The response text or any set of values that can be turned into a
        Response object using
        `make_response <http://flask.pocoo.org/docs/1.0/api/#flask.Flask.make_response>`.
    """
    request_json = request.get_json()
    if request.args and 'message' in request.args:
        return request.args.get('message')
    elif request_json and 'message' in request_json:
        return request_json['message']
    else:
        return f'Hello World!'
```

## 使用 gcloud 命令部署函数

现在我们已经了解了如何使用 Google Cloud Console 部署函数，更容易解释 gcloud 命令的参数和标志。

要部署 Cloud Functions，我们可以使用以下命令：

```
gcloud deploy cloud functions $FUNCTION_NAME \
--region=$REGION \
--entry-point=$ENTRY_POINT \
--memory=$MEMORY \
--runtime=$RUNTIME \
--service-account=$SERVICE_ACCOUNT\
--source=$SOURCE \
--stage-bucket=$STAGE_BUCKET \
--timeout=$TIMEOUT \
--retry
```

这里，我们有以下选项：

- $REGION：函数的区域。
- $ENTRY_POINT：函数的名称（在源代码中定义）。
- $MEMORY：可使用的内存量的限制。允许的值为 128 MB、256 MB、512 MB、1,024 MB 和 2,048 MB。
- $SERVICE_ACCOUNT：与函数关联的 IAM 服务账号。
- $SOURCE：源代码的位置。可以是 Cloud Storage、源代码仓库或本地文件系统。
- $STAGE_BUCKET：如果函数从本地目录部署，它定义了源代码将被存储其中的 Cloud Storage 存储桶的名称。
- $TIMEOUT：函数的执行超时。
- --retry：仅适用于后台函数。如果存在，它定义如果函数未成功执行则应重试运行。

接下来，让我们定义我们的触发器。

## 触发器

在定义必要参数后，您可以根据希望如何启动函数来定义以下触发器。

要定义 HTTP 触发器，请使用以下命令：

```
--trigger-http
```

将为函数分配一个端点。

要触发 Cloud Storage 存储桶更改时的函数，请使用以下命令：

```
--trigger-bucket=$TRIGGER_BUCKET
```

这里，我们有以下选项：

- $TRIGGER_BUCKET：Google Cloud Storage 存储桶名称。对此存储桶中文件所做的任何更改都将触发函数执行。

要触发 Pub/Sub 队列中消息到达时的函数，请使用以下命令：

```
--trigger-topic=$TRIGGER_TOPIC
```

这里，我们有以下选项：

- $TRIGGER_TOPIC：Pub/Sub 主题的名称。队列中到达的消息将触发此函数。消息的内容将传递给函数。

对于其他来源，例如 Firebase，请使用以下命令：

```
--trigger-event=$EVENT_TYPE
```

```
--trigger-resource=$RESOURCE
```

这里，我们有以下选项：

- $EVENT_TYPE：应触发函数的动作
- $RESOURCE：事件发生的资源

让我们看一个从 Pub/Sub 配置触发器的示例：

```
gcloud functions deploy hello_pubsub --runtime python37
--trigger-topic mytopic
```

