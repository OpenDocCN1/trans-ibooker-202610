
您的凭据可能会通过镜像中的存在而暴露，这不仅构成安全威胁，还会使您更难更改它们。因此，您希望保持它们的灵活性并将其移出代码。所有服务器环境都为您提供了一种在运行时向应用程序暴露变量的方法。

对于 Ubuntu，这与 macOS 相同。因此，如果您通过安装 Ubuntu 直接运行服务器，`export` 函数将完成其所需的工作。然而，Docker 容器从 Docker 获取环境变量。根据您运行 Docker 的方式，这可能需要您查阅特定文档。对于标准 Docker，您可以通过为变量添加 `\-e` 参数来实现，如下所示：

```
docker run -e SENDGRID_API_KEY="SG.IoE" -p 8080:8080 myservice
```

`SENDGRID_API_KEY` 是示例变量。

现在我们已经了解了如何将配置传递给服务器，让我们总结本章。

# 总结

开始吧！在本章中，我们了解了 Swift 在服务端的现状。我们从通用的微服务结构入手，了解了其样貌。在此过程中，我们从单服务器系统过渡到多服务器系统，包括负载均衡器。

随后，我们探讨了如何在 Linux 系统上运行 Vapor 应用。我们完整走过了 Swift 安装过程，接着了解了 Docker 如何简化安装过程。最后，我们研究了配置应如何传递给服务器应用，以及这在 macOS、Linux 和 Docker 上的表现。

在本章中，您了解了 Swift 在服务端的地位。您学会了如何在 Ubuntu 上安装 Swift 以及如何通过 Docker 运行它。掌握这些技能后，您基本上可以在 Linux 服务器上运行任何 Swift 应用。

既然您已掌握使用 Swift 运行服务器应用的基础，下一章我们将把应用部署到云端！

# 问题

为确保您对本章的理解扎实，请回答以下问题：

1.  为什么不建议仅在一台服务器上运行整个后端？

2.  Docker 如何简化您的安装过程？

3.  您应该如何在数据库中存储凭据？

4.  负载均衡器扮演什么角色？

# Docker 与云

在部署微服务时，Docker 无疑是最常用、最流行的技术，帮助我们启动服务。我们在 [第 13 章](#ded0204f-ec98-40eb-8e40-583d10486a93.xhtml) *托管微服务* 中介绍了 Docker，在本章中，我们将深入探讨。首先，我们将深入理解 Docker 本身及其运行方式。然后，我们将更好地了解在不同云提供商上运行 Docker 的情况。

学完本章后，您将确切了解 Docker 如何与服务端 Swift 结合，以及不同云提供商如何处理 Docker。

本章将涵盖以下主题：

*   探索 Docker 容器与镜像：了解 Docker 的核心
*   在 AWS 上运行 Docker：最流行的云提供商
*   在 Google Cloud 上运行 Docker：运行可扩展后端的另一种选择
*   在 Digital Ocean 上运行 Docker：使用 Digital Ocean 启动您的后端
*   在 Heroku 上运行 Docker：使用 Heroku 运行您的后端
*   使用 Kubernetes：一种良好管理 Docker 的方式

让我们从 Docker 基础开始！

# 技术要求

本节介绍您需要在机器上运行的组件，以便编写本章的微服务。需要安装以下组件：

*   `Vapor 4`
*   `Swift 5`
*   `XCode 11+`
*   `Docker`
*   `macOS/Linux`
*   `AWS CLI`
*   `Google Cloud CLI`
*   `Docker Machine`

我们还假设您已学完前几章，知道如何编写 Vapor 微服务。本章所有代码在 macOS 和 Linux 下运行方式相同。

对于本章，您可能想注册一个您选择的云提供商账号。我们将介绍最流行的几个，但如果您已有账号，将更容易跟随操作。

本章的 GitHub 仓库：[https://github.com/PacktPublishing/Hands-On-Swift-5-Microservices-Development/tree/master/Chapter 14](https://github.com/PacktPublishing/Hands-On-Swift-5-Microservices-Development/tree/master/Chapter%2014)。

# 探索 Docker 容器与镜像

此时，您的机器上应已安装 Docker。如果没有，请访问 [https://docs.docker.com/install/](https://docs.docker.com/install/) 并按照您系统的说明操作。在本节中，我们将探索 Docker 的核心，以更好地理解其运作方式。Docker 拥有镜像（编译后的系统状态）和容器（运行时的系统状态）。两者都是 Docker 交互的核心单元。

本节将涵盖以下主题：

*   设置 Docker：我们将确保 Docker 已正确安装。
*   镜像、仓库与容器：理解它们的区别与用途。
*   启动 Hello World：来自 Docker 的问候。

让我们从设置 Docker 开始。

# 设置 Docker

让我们确保您的系统已正确安装 Docker，并且您可以继续后续步骤。如果您尚未安装 Docker，请访问 [https://docs.docker.com/install/](https://docs.docker.com/install/) 并在您的机器上安装它。

打开终端（或控制台）并运行以下命令：

```
docker version
```

您的输出现在应显示版本，如下所示：

```
Client: Docker Engine - Community
 Version:           19.03.5
 API version:       1.40
 Go version:        go1.12.12
 Git commit:        633a0ea
 Built:             Wed Nov 13 07:22:34 2019
 OS/Arch:           darwin/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.5
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.12
  Git commit:       633a0ea
  Built:            Wed Nov 13 07:29:19 2019
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v1.2.10
  GitCommit:        b34a5c8af56e510852c35414db4c1f4fa6172339
 runc:
  Version:          1.0.0-rc8+dev
  GitCommit:        3e425f80a8c931f88e6d94a8c831b9d5aa481657
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```

请确保安装了 Docker 19 或更新版本。当您运行 Docker 时，实际上使用的是 Docker 客户端。在 macOS 上，服务器是 Docker Desktop 的一部分，后者实际完成了大部分工作。请确保 Docker Desktop 也已启动并运行。如果已运行，您应该会在顶部栏看到一个小图标，如下所示：

![](assets/75eb9bee-afc0-4de6-9747-607f7412428e.png)

现在，我们已准备好使用 Docker，让我们开始了解镜像和容器。

# 镜像、仓库与容器

我们在 [第 2 章](#0437cb27-3342-4d58-8253-7093af9626ac.xhtml) *理解服务端 Swift* 中简要讨论了镜像和容器。在本节中，我们将讨论 Docker 的三个主要元素：

*   仓库
*   镜像
*   容器

让我们从仓库开始。

# 仓库

您可能从使用 Git 或其他版本控制工具中了解什么是仓库。对于 Docker 也是如此。仓库保存 Docker 镜像的不同版本。当您构建 Docker 镜像时，它会本地存储在构建它的计算机上。然后，我们可以将其推送到仓库，在那里它被保存和版本控制。

从该仓库，当我们想将其作为容器运行时，可以再次拉取它。

下图解释了镜像和仓库是如何连接的：

![](assets/da4526a1-022a-4bbf-b15f-520616466f15.png)

现在，让我们看看镜像。

# 镜像
