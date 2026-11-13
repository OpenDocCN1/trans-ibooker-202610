# 应用程序结构和数据库设计

在你为服务创建了自己的模板后，现在让我们设置整个项目。本章将从实践角度为你做好准备，以便你能着手处理后端结构。对于涉及同时运行多个应用程序（每个服务上一个）的微服务应用程序，其设置与单体开发截然不同：在单体应用程序中，你只需测试一个应用程序即可测试所有内容。在微服务设置中，你需要测试多个相互交互的微服务。

本章的核心是让你的系统为微服务开发做好准备。你将学习应用程序应如何构建以及如何设计数据库。我们将在本章中进行大量实践操作，设置后续章节所需的文件夹。你还将学习如何设置 Git 和 Docker，这是微服务的必备工具。最后但同样重要的是，你将学习如何在你的机器上设置 Vapor、数据库和我们的服务。这些准备工作将在下一章开发微服务时发挥作用，因为你的系统已经准备就绪。

我们将探讨以下方面：

*   理解项目设置和文件夹结构
*   安装 Git 和 Docker
*   设置 Vapor 和数据库
*   设置我们的服务

让我们开始吧！

## 技术要求

要完成本章，你应该具备以下设置：

*   已安装 Swift 5
*   已安装 Vapor Toolbox
*   已安装 Xcode 11+
*   macOS
*   对于 Docker，你需要以下内容：
    *   Mac 硬件必须是 2010 年或更新的机型，带有 Intel 硬件
    *   支持 macOS Sierra 10.12 及更新版本
    *   至少 4 GB 内存
    *   **不得**安装 4.3.30 版本之前的 VirtualBox

虽然所有这些在 Linux 上同样适用，但本章中的插图将展示 macOS。

本章的代码文件可以在这里找到：[`github.com/PacktPublishing/Hands-On-Swift-5-Microservices-Development/tree/master/Chapter 6/Shop Backend/template`](https://github.com/PacktPublishing/Hands-On-Swift-5-Microservices-Development/tree/master/Chapter%206/Shop%20Backend/template)。

## 理解项目设置和文件夹结构

让我们首先为项目设置文件夹。对于微服务项目来说，拥有整洁的文件夹设置以保持一切井井有条非常重要。我们需要为以下微服务创建文件夹：

*   `OrderService`：我们的订单处理服务，负责处理支付和保存订单
*   `ProductService`：我们的产品服务，负责提供产品信息
*   `UserService`：我们的用户服务，负责管理用户账户

请遵循以下步骤：

1.  为项目创建一个新文件夹并将其命名为 `Shop Backend`。你可以将此文件夹放在电脑上的任何位置。你希望将所有内容组织在开发机器上的一个中心位置。
2.  创建一个 `template` 文件夹，并将我们在第 5 章 *Creating Your First Microservice* 中创建的模板代码移入其中。
3.  创建三个名为 `OrderService`、`ProductService` 和 `UserService` 的空文件夹。

最后，文件夹结构应该如下所示：

![](img/93e1cabb-575c-4f5a-9292-82d718cd776a.png)

这主要基于个人偏好，但我倾向于不在实际项目文件夹中使用空格。如今任何计算机都能处理空格，但在使用终端或控制台时显示得不太美观，会显示为 `Order\ Service`。

4.  在将模板复制到 template 文件夹后，继续将模板复制到另外三个文件夹中。你可能会想知道为什么我们需要在那么多地方复制相同的代码。我们将修改这些代码并使用它们。在某些情况下更改可能很小，但你希望保持其有序整洁，即使代码重复多次，也不愿将太多东西组合在一起导致混乱（相信我，我自己经常这样做）。
5.  进入控制台并打开项目文件夹。如果你不习惯大量使用控制台，以下命令将帮助你：

| 命令 | 说明 |
| --- | --- |
| `ls` | 列出当前文件夹中的所有文件和文件夹 |
| `cd <foldername>` | 根据给定名称打开文件夹 |
| `pwd` | 打印出你当前所在的文件夹 |
| `cd` (无参数) | 进入主文件夹 |

6.  进入 `Shop Backend` 文件夹，然后在每个项目文件夹中执行 `xed .` 命令。此命令将在 Xcode 中打开项目。处理服务时你将大量使用终端/控制台，所以请习惯它！

现在文件已经设置好了，让我们设置 Git 和 Docker，以便我们能妥善保存代码并通过 Docker 发布应用程序。

## 安装 Git 和 Docker

Git 和 Docker 是我们将用来管理代码和构建的两个必备工具（除了本地机器）。在本节中，我们将安装它们并探索它们如何帮助我们。

我们将探讨以下主题：

*   安装 Git
*   安装 Docker
*   在微服务中使用 Docker
*   使用 Git 和 Docker

让我们开始吧！

### 安装 Git

幸运的是，如果你使用的是 macOS，Git 很可能已经安装了。你可以通过在终端中输入以下命令来验证这一点：

```
$ git --version
```

结果应该如下所示：

```
$ git --version
git version 2.21.0 (Apple Git-120)
```



如果看起来不像这样，你可能需要安装 Git，前面的命令会提示你进行安装。安装 Xcode（到这一步你应该已经完成了）时，Git 也会一并安装。

如果你使用的是 Linux 并且需要安装 Git，只需运行 `sudo apt install git-all` 或 `sudo dnf install git-all` 即可，具体取决于你的 Linux 发行版。

就这样——很简单，对吧？现在，让我们安装 Docker！

# 安装 Docker

与 Git 不同，Docker 不会随 Xcode 自动安装。要安装 Docker，请执行以下步骤：

1. 打开 `https://hub.docker.com/editions/community/docker-ce-desktop-mac` 并下载 `Docker for Mac`。你需要注册才能下载 Docker。

2. 双击 `Docker.dmg` 并完成安装过程。安装完成后，你会在顶部菜单栏看到 Docker 图标：

![](img/fd8df3de-b8dd-4dc1-83ac-22cacbc86cba.png)

3. 在 `Terminal` 中运行 `docker version` 验证一切是否设置正确。输出应如下所示：

![](img/aff5ef3e-8836-471a-bf6e-e2c4b6bd1e99.png)

Docker 就安装好了！让我们探讨一下为什么 Docker 和微服务联系如此紧密。

# 将 Docker 与微服务结合使用

Docker 是一种允许应用程序在虚拟系统中运行的工具，因此 Linux 应用程序可以通过 Docker 在 macOS 或 Windows 下运行。Docker 在内核层面虚拟化操作系统，如果你熟悉 `VirtualBox` 的话，这比它的性能高得多。

能够在无论实际宿主系统如何的情况下运行应用程序，这对服务器来说显然是一个非常有用的功能。现在我们可以在几乎任何硬件上运行我们的应用程序！但这还不止于此。

Docker 允许我们为应用程序配置一个虚拟系统。我们可以指定需要安装哪些程序、需要什么权限，以及如何设置所有内容。Docker 启动应用程序，一切都完全按照我们定义的方式运行（在所谓的 `Dockerfile` 中）。这是微服务的一项非常重要的功能。请记住，微服务被设计为独立运行。这也意味着我们希望能随意地对它们进行扩缩容。如果我们想启动几百个服务实例，不应该去逐个配置它们。而 Docker 就是解决这个问题的方案；有了它，我们只需定义一次系统设置，就可以随心所欲地启动任意多次。

在探讨了用于微服务的 Docker 之后，让我们在项目背景下看看 Git 和 Docker。

# 配合 AWS 和 GitHub 设置 Docker 和 Git

到目前为止，你应该已经安装了 Git 和 Docker。关于 Git 和 Docker，以下两条规则对微服务至关重要：

* 每个服务都需要自己的 Git 仓库。
* 每个服务都需要自己的 Docker 仓库。

Docker 就像 Git 一样，拥有保存构建历史和当前版本的仓库。你的 CI 将使用 Docker 仓库来更新微服务：

1. 继续设置你的仓库。如果你需要一个起点，请查看 AWS `ECR`：[`aws.amazon.com/ecr/`](https://aws.amazon.com/ecr/)

2. 为你的 Git 仓库设置一个 GitHub 账号（[`github.com/`](https://github.com/)）。

3. 设置一个 AWS 账号来托管你的项目。

你可以使用任何其他“兼容 Git 和 Docker”的服务；根据我的经验，AWS 和 GitHub 是易于上手且便于扩展的良好起点。在 AWS 中，你可以在 AWS `ECR`（`Elastic Container Registry`，弹性容器注册表）中设置仓库。

一旦你设置好 AWS 和 GitHub，你的项目就可以同时连接到 GitHub 和 AWS，但我们将在第 14 章 *Docker and the Cloud* 中介绍这一点。

现在你已经为 GitHub 和 AWS 打好了基础，让我们看看 Vapor 和数据库的设置。

# 设置 Vapor 和数据库

此时，Swift 和 Vapor 应该已经安装并可正常工作。如果没有，请返回第 3 章。剩下的就是设置你的本地数据库环境。既然我们已经在使用 Docker，就可以轻松地通过 Docker 启动一个 MySQL 数据库。请遵循以下步骤：

1. 进入终端并运行以下命令：

```
sudo mkdir /usr/local/opt/mysql
sudo mkdir /usr/local/opt/mysql/8.0
sudo chmod -R ug+w /usr/local/opt/mysql/
```

通过执行上述命令，我们正在安装一个 Docker 实例可以访问的本地数据库文件夹。这可以防止我们在关闭 Docker 时丢失数据。

2. 现在，授予 Docker 与该文件夹共享信息的权限。打开 Docker 的偏好设置：

![](img/8b9af91f-92a4-48a6-8430-588ee865a794.png)

3. 将我们刚创建的文件夹添加到“文件共享”下的文件列表中，然后点击“Apply & Restart”（应用并重启）：

![](img/d78338ba-a062-47a1-9dbd-371384dff24b.png)

4. 现在，我们可以启动 MySQL 服务器，将 `your_password` 替换为你更满意的密码。这是一个本地服务器，我们仅将其用于开发目的。请参阅以下示例：

```
docker run --restart always --name mysql8.0 -v /usr/local/opt/mysql/8.0:/var/lib/mysql -p 3306:3306 -d -e MYSQL_ROOT_PASSWORD=your_password mysql:8.0
```

5. 让我们使用以下代码设置三个用户和三个数据库：

```
mysql -u root -p
mysql> CREATE USER 'service_users'@'localhost' IDENTIFIED BY 'password';
mysql> CREATE USER 'service_products'@'localhost' IDENTIFIED BY 'password';
mysql> CREATE USER 'service_orders'@'localhost' IDENTIFIED BY 'password';
mysql> CREATE DATABASE 'service_users';
mysql> CREATE DATABASE 'service_products';
mysql> CREATE DATABASE 'service_orders';
mysql> GRANT ALL PRIVILEGES ON service_users . * TO 'service_users'@'localhost';
mysql> GRANT ALL PRIVILEGES ON service_products . * TO 'service_products'@'localhost';
mysql> GRANT ALL PRIVILEGES ON service_orders . * TO 'service_orders'@'localhost';
mysql> FLUSH PRIVILEGES;
```

上述代码为你的数据库创建了三个用户、三个数据库，并授予用户访问这些数据库的权限。每个微服务将拥有自己的用户和数据库，这一步确保它们已经存在。

6. 请务必将密码和用户名记录在安全的密码管理器中。这仅用于开发目的，所以你可以（也许应该？）使用容易记住的密码。

现在你已经设置好数据库，我们可以设置各个独立服务了。

# 设置我们的服务

正如前几章所见，我们的后端需要设置三个服务。现在是设置这三个服务的时候了。本节包含三个部分：

* 设置 Dockerfile
* 设置环境变量
* 运行整个应用程序

## 设置 Dockerfile

`Dockerfile` 被 Docker 用来创建新镜像。它包含你希望 Docker 执行以获得最终镜像的指令。要设置 Dockerfile，请遵循以下步骤：

1. 进入每个服务文件夹并打开 `Package.swift` 文件。

2. 相应地命名服务（`UserManager`、`ProductManager` 或 `OrderManager`）。

3. 现在在每个文件夹中创建一个包含以下命令的 `Dockerfile` 文件：



```
FROM swift:5.1.2 as builder

# For local build, add `--build-arg env=docker`
# In your application, you can use `Environment.custom(name: "docker")` to check if you're in this env

RUN apt-get -qq update && apt-get install -y \
  libssl-dev zlib1g-dev \
  && rm -r /var/lib/apt/lists/*

ARG env
RUN mkdir /root/.ssh/
RUN apt-get -qq update && apt-get install -y \
libssl-dev zlib1g-dev \
&& rm -r /var/lib/apt/lists/*
WORKDIR /app
COPY . .
RUN mkdir -p /build/lib && cp -R /usr/lib/swift/linux/*.so* /build/lib
RUN swift package --enable-pubgrub-resolver resolve && \
    swift build -c release --verbose && \
    mv `swift build -c release --show-bin-path` /build/bin

# Production image
FROM ubuntu:18.04
ARG env
# DEBIAN_FRONTEND=noninteractive for automatic UTC configuration
in tzdata
RUN apt-get -qq update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
libatomic1 libicu60 libxml2 libcurl4 libz-dev libbsd0 tzdata \
&& rm -r /var/lib/apt/lists/*
WORKDIR /app
COPY --from=builder /build/bin/Run .
#COPY --from=builder /build/bin/*.so /usr/lib/
COPY --from=builder /build/lib/* /usr/lib/

ENTRYPOINT ./Run serve --hostname 0.0.0.0 --port 8080
```

前面的文件包含在生产环境中运行服务所需的一切。因此，当 Docker 使用此文件生成可启动为服务器的镜像时，该文件为 Docker 提供了运行我们微服务所需的一切。每个服务的文件可以相同；没有区别。

此 Dockerfile 将源代码保留在镜像中。在某些情况下，您可能想要编译可执行文件，然后仅在不包含源代码的 Docker 镜像中使用它。无论何种情况，您都希望保护每个 Docker 容器的安全，但不包含源代码可能出于其他安全相关原因。

大多数基于 Vapor 模板的 Vapor 项目都包含一个 `web.Dockerfile`，它等同于我们上面粘贴的内容；您可以选择使用那个，或者使用我们上面使用的这个。

## 设置环境变量

为了能够成功启动每个服务，我们需要将正确的凭据传递给应用程序。Vapor 使用环境变量，它们存储在终端的会话中。因此，要为特定服务设置您的终端，您将执行以下操作：

```
export MYSQL_CRED="mysql://service_users:your_password@localhost:3306/service_users?ssl=false"
export JWKS="JWKS-Data"
```

通过前六条语句，您正在设置每个服务所需的环境变量。还记得第一章的 JWT 吗？我们现在以 JWKS 的形式设置 JWT 数据，并将值传递给此处的 `JWKS` 环境变量。您将在*第 7 章：编写用户服务*中看到其实际运行情况。

将这些命令保存在一个文本文件中以便打开、复制和粘贴是合理的。Vapor 4 使用 `.env` 文件进行此设置，这与其他框架类似。

## 运行整个应用程序

现在，我们如何一次性运行这三个服务，您可能会问？本质上，您需要三个终端实例——每个服务一个。然后您可以在单独的会话中运行每个服务。唯一需要考虑的是，由于您在同一台机器上运行它们，每个服务需要不同的端口。您可以通过像这样运行每个服务来实现：

```
./.build/debug/Run serve --port=8080
./.build/debug/Run serve --port=8081
```

这将同时完美启动所有服务。现在您已经设置了 Docker 和环境变量，并准备好同时运行两个应用程序。

## 总结

在本章中，您设置了在线商店后端文件夹所需的一切。您基于之前生成的模板创建了三个项目，并设置了数据库。

您了解了 Docker 和 Git 是微服务开发的必备工具。两者都已设置好，您可能已经拥有 AWS 账户（或其他云提供商）。您还学习了如何设置 Dockerfile，并为我们的示例项目完成了设置。

本章您所学和所做的一切都是为了在下一章中编写示例项目的第一个微服务所需的准备工作。

## 问题

通过回答以下问题来检查您的理解：

1.  为什么为每个服务使用不同的 Git 和 Docker 仓库至关重要？
2.  您的服务如何接收数据库和 JWT 的凭据？
3.  如何同时运行多个 Vapor 应用程序？
4.  进阶：将您的 Docker 镜像推送到您的 Docker 仓库。
