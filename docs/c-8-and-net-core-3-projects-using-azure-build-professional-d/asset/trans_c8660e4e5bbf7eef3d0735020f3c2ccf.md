

var message = await queue.GetMessageAsync();
if (message == null) return null;
string data = message.AsString;
var salesOrder =
JsonConvert.DeserializeObject<Models.SalesOrder>(data);

await queue.DeleteMessageAsync(message.Id, message.PopReceipt);
return salesOrder;
}
```

再次，这是一组非常熟悉的方法；重点如下：

```
var message = await queue.GetMessageAsync();
...
string data = message.AsString;
```

这只是返回一条消息。现在，我们可以通过使用 AsString 属性来提取其内容。然后，我们将其反序列化回 SalesOrder 对象：

```
await queue.DeleteMessageAsync(message.Id, message.PopReceipt);
```

此命令从队列中删除消息。值得注意的是，在此之后如果发生崩溃，消息将丢失。你可能需要考虑保留此 ID，甚至在创建订单后才删除消息。不过，我们仍然可以识别出唯一的订单，因为我们能够确定销售订单没有确认。

如果现在运行此项目，你会发现它按预期工作，并开始处理队列中的任何消息。

最后一步是将此上传到 Azure 的 AKS 系统，从而完成项目。

现在我们已创建项目，让我们看看如何在 Microsoft Azure Kubernetes Service 中托管该项目。

## Azure Kubernetes 服务

Azure Kubernetes Service（通常简称为 AKS）只是多种云端 Kubernetes 产品之一。它介于创建和托管自己的 Kubernetes 集群与使用特定云服务（例如 AWS Lambda 和 Azure Functions）之间，是一种半成品解决方案。主要优势在于控制力（你可以确定部署如何以及为何进行缩放）和可移植性（你应该能够从 Microsoft 迁出，将完全相同的设置部署到 Google，并从中断处继续）。

## 构建 Docker 镜像

让我们从代码创建 Docker 镜像。在项目中找到 Dockerfile，并从上下文菜单中选择“构建 Docker 图像”：

![](img/762492a8287f369cc1ac4621a212f193_397_0.png)

## Azure 容器注册表

现在，你应该能够打开 PowerShell 并输入以下内容：

```
docker images
```

这将列出你当前机器上所有的 Docker 镜像。容器注册表本质上是此的远程版本；也就是说，你创建镜像并将其上传到注册表。当上传到 AKS 时，我们需要镜像位于容器注册表中。虽然 Azure 有自己的容器注册表，但你可以使用任何注册表；Docker 拥有容器注册表，大多数（如果不是全部）大型云提供商也都有。在本节中，我们将上传到 **Azure 容器注册表 (ACR)**。

直到现在，我们一直依赖 Visual Studio 来管理 Docker 镜像；撰写本文时，这是一项非常新的功能，以前所有操作都需要在命令行中完成。现在，我们需要离开 IDE 的舒适区，进入 PowerShell 和 Azure CLI 的晦涩世界。为确保已安装 CLI，请打开 PowerShell 窗口并输入以下命令：

```
az --version
```

> 我的版本是 2.0.57，不过 2.0.55 及更高版本应该都可以。如果你的版本低于该版本，请返回 *技术要求* 部分并重新安装 CLI。如果你未安装此工具，请跳回 *技术要求* 部分，那里有安装说明。

我们的首要任务是将镜像推送到 ACR。

## Azure 容器注册表

现在我们已确认拥有有效的 CLI 版本，可以创建新的注册表；以下命令将为你完成此操作：

```
az acr create --resource-group netcode-projects --name salesorderregistry --sku Basic
```

netcode-projects 是我的资源组，因此你需要将其替换为自己的资源组。我将注册表命名为 salesorderregistry，但只要你遵守命名规则，就可以随意命名你的注册表。sku 决定可用的性能和规模。

使用时你需要查阅具体细节，但大致来说，付费越多，性能越好，规模也越容易扩展。这是云使用中的常见模式。

上述命令的响应应类似于以下内容：

```
PS C:\> az acr create --resource-group netcode-projects --name salesorderregistry --sku Basic
{
  "adminUserEnabled": false,
  "creationDate": "2019-04-10T07:13:45.7357544+00:00",
  "id": "/subscriptions/.../resourceGroups/netcode-projects/providers/Microsoft.ContainerRegistry/registries/salesorderregistry",
  "location": "uksouth",
  "loginServer": "salesorderregistry.azurecr.io",
  "name": "salesorderregistry",
  "networkRuleSet": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "netcode-projects",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

请记下 loginServer 响应，因为这决定了你今后如何引用你的注册表，从登录开始：

```
az acr login --name salesorderregistry
```

在上述命令中，salesorderregistry 实际上是我们几分钟前创建 ACR 时返回的值（为此命令，我们只需要该值的第一部分）。你应该会收到此命令的响应，告知登录已成功。

此前，我们查看了 Docker 镜像以识别本地系统中存在的镜像；我们需要再次使用该命令，但这次需要记下镜像名称和标签。提醒一下，命令如下：

```
docker images
```

响应应类似于以下内容：

```
PS C:\> docker images
REPOSITORY                                      TAG                 IMAGE ID            CREATED             SIZE
D
salesorderprocess                               dev                 3071c2
                                                41 hours ago        183MB
fcfee                                           latest              e95e574
salesorderprocess                               41 hours ago        192MB
5e62a
```

首先，我们需要为镜像打标签；以下命令应能完成此操作：

```
docker tag salesorderprocess
salesorderregistry.azurecr.io/salesorderprocess:v1
```

如果再次列出镜像，应该能看到新标签：

```
salesorderregistry.azurecr.io/salesorderprocess    v1     e95e574
5e62a      41 hours ago   192MB
```

现在我们有了标签，可以将镜像推送到仓库：

```
docker push salesorderregistry.azurecr.io/salesorderprocess:v1
```

这次，我们应该会看到一些活动；也就是说，当镜像被推送到仓库时，你应该能看到状态条在移动：

```
PS C:\> docker push salesorderregistry.azurecr.io/salesorderprocess:v1
The push refers to repository [salesorderregistry.azurecr.io/salesorderprocess]
e3015455a998c: Pushed
14a21ca053e7: Pushed
28faef933b8f: Pushed
132c8212430d: Pushed
98bd9d81b9b02: Pushed
5dacd731af1b: Pushing [==================================================>]  44.47MB/55.28MB
```

为确认这一点，请使用以下命令，它应会告诉你镜像现已确实位于 ACR 中：

```
az acr repository list --name salesorderregistry --output table
```

现在我们已在容器注册表中拥有镜像，可以创建 Kubernetes 集群并设置编排。

## Azure Kubernetes 服务

我们将花费整个部分在 PowerShell 中。为了使用 Kubernetes，首先需要安装 CLI。为此，请登录 Azure：

```
az login
```

这应该会弹出一个窗口，允许你输入 Azure 凭据。身份验证通过后，你应该能够安装 Kubernetes CLI：

```
az aks install-cli
```

这可能需要下载一段时间；下载完成后，我们就可以创建新的 Kubernetes 集群。集群，本质上，就是你提到 Kubernetes 时所想到的东西。集群中的节点由主节点管理。这意味着你与主节点交互，由主节点将工作负载委派给节点。一个很好的理解方式是将其比作建筑工地上的工作队：你可以走向工头并请他建一堵墙，但他不太可能亲自去做；相反，他会找一些可用且能够建墙的工人，请他们开始施工。

以下命令应会创建我们的集群：

```
az aks create --resource-group netcode-projects --name salesorder-cluster --generate-ssh-keys
```

![](img/762492a8287f369cc1ac4621a212f193_401_0.png)

值得指出的是，创建 Kubernetes 集群会创建多个虚拟机，无论你是否实际使用它们，都需要付费。总的来说，完成本章时可能只需花费一两英镑，但请记得事后清理；否则，你可能会收到一笔出乎意料的高额账单！

同样，你应将资源组替换为本项目使用的资源组。你也可以将名称替换为……嗯，你喜欢的任何名称。

![](img/762492a8287f369cc1ac4621a212f193_401_1.png)

你请求的 SSH 密钥将允许你访问集群。

完成后，你应该会收到一个包含大量集群详细信息的 JSON 响应。

