
![](img/fb3bebce7b5f593bca1a7f8a1371be06_505_0.png)

## 开发云原生应用

Cloud Shell 将 Cloud Code 的所有优势带到您的浏览器中。您可以在浏览器中创建 Kubernetes 和 Cloud Run 应用，进行开发、调试和部署，并监控和管理您的云资源。

## 配置您的 Cloud Shell 机器

连接到您的 Cloud Shell 实例

连接到您的 Cloud Shell 编辑器

3. 完成后，您应该会看到 IDE 已加载，工作区已准备好并包含克隆的仓库。在右侧窗格中，您可以看到教程。

图 14.24 Anthos Sandbox：主屏幕

应用代码。

## 14.3.1 Cloud Source Repositories

Cloud Source Repositories[^21] 是托管在 Google Cloud 上的功能完备的私有 Git 仓库。该服务帮助开发者在 Google Cloud Platform 上私有托管、跟踪和管理大型代码库的变更。它旨在轻松与 GCP 服务集成，例如 Anthos、GKE、Cloud Run、AppEngine 和 Cloud Functions。您可以配置无限数量的仓库，还可以镜像 Bitbucket 和 GitHub 仓库。Cloud Source Repositories 中的变更会受到监控，并可触发事件通知发送到 Cloud Pub/Sub 或 Cloud Function。Cloud Source Repositories 的差异化特性之一是，您可以使用正则表达式在仓库中搜索[^22]短语。Cloud Source Repository 的审计日志可在 Cloud Operations 中获取，因此您始终可以知道谁在何时访问了您的仓库。

**图 14.25 Cloud Source Repositories 集成**

要创建代码仓库，请运行以下命令：

```
```bash
gcloud init
```
```

这将初始化您的 gcloud 命令行工具。

```
```bash
gcloud source repos create [REPO_NAME]
```
```

这将创建一个名为 REPO_NAME 的仓库。

现在仓库已准备好使用，您需要选择以下三种身份验证方法之一：

- SSH
- Cloud SDK
- 手动生成的凭据

请参阅以下链接查看如何操作的分步指南：
[https://cloud.google.com/source-repositories/docs/authentication](https://cloud.google.com/source-repositories/docs/authentication)

仓库设置完成并且您成功通过身份验证后，您可以使用 Git 命令（如 `git clone`、`git pull` 和 `git push`）与之交互。

## 14.3.2 Artifact Registry

Artifact Registry 是 Google Container Registry 服务的下一代产品。除了能够存储容器镜像之外，Artifact Registry 还可以存储其他包，例如 Maven[23]、npm[24] 和 Python[25]，并且很快将推出更多功能。这些服务与 Google Cloud Platform 生态系统完全集成。这意味着您可以使用 IAM 策略控制对工件的访问，访问 Cloud Source Repositories，使用 Cloud Build 触发自动构建，并部署到 Google Kubernetes Engine、App Engine 和 Cloud Functions。

您可以在离工作负载最近的区域创建 Artifact Repositories，以便利用高速 Google 网络拉取工件。从安全角度来看，可以扫描您的容器以查找漏洞，并使用 Binary Authorization 来批准可以推送到生产环境的镜像。

您可以使用原生工具与 Artifact Repositories 交互，因此很容易将其集成到 CI/CD 管道中。

## 将 Artifact Registry 与 Docker 结合使用

让我们看看如何与 Artifact Registry 交互来存储容器镜像。

- 1. 首先，运行以下命令创建一个 Artifact 仓库：
- 2. 您可以运行以下命令列出您的仓库：
- 3. 在推送镜像之前，您应运行以下命令向仓库进行身份验证：
- 4. 现在，出于演示目的，我们将直接从 Docker Hub 拉取官方 alpine 镜像，而不是构建新镜像：
- 5. 我们将通过运行以下命令为镜像打上仓库名称标签：
- 6. 最后，我们可以通过运行以下命令将镜像推送到 Artifact Registry：

```
gcloud artifacts repositories create quickstart-docker-repo --rep --location=us-central1 [--description="Docker repository"]
```

```
gcloud artifacts repositories list
```

```
gcloud auth configure-docker us-central1-docker.pkg.dev
```

```
docker pull alpine
```

```
docker tag alpine us-central1-docker.pkg.dev/PROJECT/quickstart-d
```

```
docker push us-central1-docker.pkg.dev/PROJECT/quickstart-docker-
```

现在您就可以从镜像仓库中拉取容器镜像了。

## Artifact Registry 总结

在撰写本书时，Artifact Registry 已正式发布（Generally Available），但某些功能可能仍处于预览阶段。作为 Container Registry 的继任者，它最终将成为 GCP 中唯一可用的容器镜像仓库。这意味着您所有的新项目都应该已经开始使用 Artifact Registry。关于现有项目如何迁移到 Artifact Registry 的分步教程可以在这里找到：

[https://cloud.google.com/artifact-registry/docs/transition/transition-from-gcr](https://cloud.google.com/artifact-registry/docs/transition/transition-from-gcr)

## 14.3.3 Cloud Build

我们已经了解了如何对 Anthos 应用代码进行版本控制以及如何构建容器镜像。现在让我们看看如何创建持续集成管道。

Cloud Build 是 GCP 原生的 CI/CD 平台，是 Gitlab CI/CD、Jenkins 或 CircleCI 等工具的替代方案。它允许您在所有 Google 计算服务（包括 Anthos GKE 和 Cloud Run for Anthos）上部署、测试和构建应用。Cloud Build 管道的步骤以容器形式运行：

![](img/fb3bebce7b5f593bca1a7f8a1371be06_511_0.png)

通过直接访问仓库、镜像仓库和工作负载来缩短部署时间。您还可以将 Cloud Build 与 **Spinnaker**[26] 等工具结合使用，以执行包含各种部署场景的更复杂管道。

Cloud Build 管道可以通过手动触发，也可以通过代码仓库的拉取请求触发。

既然我们已经了解了 Cloud Build 的基本工作原理，接下来让我们看看 Cloud Builders。

## Cloud Builders

正如我们已经了解的，Cloud Build 会运行 `cloudbuild.yaml` 文件中定义的一系列步骤。这些步骤在容器内执行。这些容器使用每个步骤的 name 属性中定义的容器镜像进行部署。这些容器镜像被称为 Cloud Builders。它们是经过特殊打包的镜像，可使用一组用户定义的属性来运行特定工具（如 docker、git 或 kubectl 等）。有三种类型的构建器。

- Google 支持的构建器
- 社区支持的构建器
- 自定义开发的构建器

让我们逐一了解每种类型：

## Google 支持的构建器

您可以在 Github 上找到 Google 支持的构建器完整列表：[https://github.com/GoogleCloudPlatform/cloud-builders](https://github.com/GoogleCloudPlatform/cloud-builders)。所有镜像都可以在容器镜像仓库中获取，路径为：gcr.io/cloud-builders/<builder name>。在 Anthos 环境下，一些最重要的构建器包括：

- docker
- git
- gcloud
- gke-deploy
- kubectl

## 社区支持的构建器

如果没有符合您需求的官方构建器，您可以使用社区构建器。有一些构建器可配合 *Helm*、*Packer*、*Skaffold*、*Terraform*、*Vault* 等工具使用。完整的社区 Cloud 构建器列表可以在这里找到：
https://github.com/GoogleCloudPlatform/cloud-builders-community

## 自定义开发的构建器

您可以创建自己的自定义构建器，用于您的构建中。自定义构建器是一个容器镜像，Cloud Build 会拉取该镜像并与您的源代码一起运行。您的自定义构建器可以执行容器内的任何脚本或二进制文件，因此它可以做容器能做的任何事情。有关创建自定义构建器的说明，请参阅：
https://cloud.google.com/cloud-build/docs/create-custom-build-steps

## 构建容器镜像

您可以使用配置文件或仅使用 Dockerfile 通过 Cloud Build 构建容器。让我们看看每种选项：

## 使用配置文件构建容器镜像

构建容器的第一种方法要求您提供 `cloudbuild.yaml` 配置文件作为输入：

图 14.27 使用 Cloud Build 配置文件构建容器镜像

