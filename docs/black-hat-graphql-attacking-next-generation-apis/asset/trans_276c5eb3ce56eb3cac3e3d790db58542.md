

牢记这些最佳实践，让我们动手搭建实验室吧！

## 安装 Kali

*Kali* 是一个专为渗透测试创建的 Linux 发行版。基于 Debian，由 Offensive Security (https://offensive-security.com) 设计。我们将使用 Kali 作为 GraphQL 攻击实验室的基础操作系统，因为它捆绑了我们所需的一些库、依赖项和工具。

你可以在 https://www.kali.org/get-kali 获取适用于 VMware Workstation 和 Oracle VirtualBox 超级管理程序的 Kali 虚拟机镜像。选择你喜欢的超级管理程序并按照 Offensive Security 提供的官方安装说明进行操作：https://www.kali.org/docs/installation。

完成安装过程后，你应该会看到图 2-1 所示的 Kali 登录屏幕。Kali 附带一个名为 *kali* 的默认用户账户，其密码为 *kali*。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_48_0.png)

图 2-1：Kali Linux 登录屏幕

登录 Kali 后，你需要确保它是最新的。打开 Kali 的 **Applications** 菜单，并在搜索栏中输入 **terminal emulator**（图 2-2）。点击相应的应用程序。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_49_0.png)

图 2-2：Kali Applications 菜单

让我们使用几个命令来更新你的软件仓库并升级已安装的软件包。在终端窗口中，输入以下命令：

```
# sudo apt update -y
# sudo apt upgrade -y
# sudo apt dist-upgrade -y
```

> NOTE 当你使用 sudo 命令时，Kali 会要求你输入密码。这与你登录虚拟机时使用的密码相同，即 kali。

从现在开始，我们将使用 Kali 机器来完成本书中涵盖的所有任务。我们建议保持终端窗口打开，因为你很快就需要它进行额外的安装。

## 安装 Web 客户端

在第 1 章中，我们提到 GraphQL API 可以使用各种专用工具进行查询，例如 GraphiQL Explorer，或简单的基于命令行 的 HTTP 客户端，例如 cURL。这些工具在幕后都会发出 HTTP 请求。

我们将安装并使用两个 Web 客户端：cURL 和 Altair。这些将允许你使用命令行工具和图形界面工具来试验构建和发送 GraphQL 查询。

## 使用 cURL 从命令行查询

最受欢迎的命令行 HTTP 客户端之一 cURL，可以像任何图形 Web 浏览器一样发出 HTTP 请求。因此，你可以使用它来查询 GraphQL API。

作为一名黑客，你应该习惯于从命令行操作。除了可以更轻松地自动化重复性任务外，熟悉命令行还能让你在没有图形界面的情况下高效工作，例如在渗透测试期间。

让我们继续安装 cURL。打开终端并输入以下命令：

```
# sudo apt install -y curl
```

你可以通过发出以下命令来验证 cURL 是否已成功安装并正常运行：

```
# curl lab.blackhatgraphql.com
Black Hat GraphQL - Hello!
```

如果看到 “Hello!” 消息，则意味着 cURL 成功向应用程序发送了 HTTP GET 请求并收到了响应。

## 使用 Altair 从 GUI 查询

在第 1 章中，我们通过使用 GraphiQL Explorer 并利用其自动补全功能来查询 GraphQL API。虽然 GraphiQL 是一个非常有用的工具，但在渗透测试期间它并不总是可用。为了解决这个问题，你可以在本地计算机上安装图形 GraphQL 客户端。这些客户端有能力连接到远程 GraphQL 服务器并返回结果，方式与 GraphiQL Explorer 类似。如果你向图形客户端提供远程服务器地址，它将在幕后处理与 GraphQL 的集成。

其中一个工具 Altair 可作为 Web 浏览器插件以及本地桌面应用程序使用。两个版本都提供相同的功能，选择其中任何一个都没有缺点。在本书中，我们将使用桌面应用程序。但是，如果你愿意，你也可以通过在浏览器地址栏中输入 about:addons 来找到的附加组件商店，为 Firefox 安装浏览器插件。

Altair 桌面客户端可在 macOS、Linux 和 Windows 上使用，地址为 https://altair.sirmuel.design/#download，如图 2-3 所示。选择代表你正在运行的操作系统的图标。对于 Kali，你需要安装 Linux 版本。

将 Altair 下载到 Kali 的 Desktop 目录。下载完成后，你应该会看到一个扩展名为 AppImage 的文件：

```
# cd ~/Desktop
# ls -l altair*
-rwxr-xr-- 1 kali kali 88819862 altair_x86_64_linux.AppImage
```

![](img/2f244b00cd9f8f37cfc22f928aaa471a_51_0.png)

图 2-3：可用的 Altair Desktop 客户端版本

接下来，我们需要更改下载文件的权限以便运行它：

```
# chmod u+x altair_x86_64_linux.AppImage
```

现在我们可以执行该文件。它应该加载客户端，如图 2-4 所示。

```
# ./altair_x86_64_linux.AppImage
```

设置正确的权限后，你还应该能够通过点击 Kali 桌面上的 Altair Desktop 图标直接运行该应用程序。

让我们现在验证客户端是否按预期工作。打开它，并在 Enter URL 地址栏中输入 http://lab.blackhatgraphql.com/graphql。这将确保我们执行的任何查询都会直接发送到此地址。现在，在左侧的 Query 窗格中，删除现有的代码注释（以 # 符号开头的行），并改输入以下查询：

```
query {
  users {
    username
  }
}
```

![](img/2f244b00cd9f8f37cfc22f928aaa471a_52_0.png)

图 2-4：适用于 Linux 的 Altair Desktop 客户端

最后，点击 Send Request。你应该会看到与图 2-5 相似的输出。

![](img/2f244b00cd9f8f37cfc22f928aaa471a_52_1.png)

图 2-5：Altair Desktop 客户端中的 GraphQL 响应

Altair 是一个功能强大的工具；它将为我们提供查询自动补全建议、架构文档、已执行查询的历史记录以及其他功能，例如设置自定义 HTTP 头和将查询保存到集合中，让我们的生活变得更轻松。要了解有关 Altair 高级功能的更多信息，请参阅官方文档页面 https://altair.sirmuel.design/docs/features。

> NOTE
> 在渗透测试期间，你可能会遇到具有身份验证和授权控制的远程 GraphQL API 服务器。像 Altair 这样的 GraphQL 客户端需要设置特殊的 HTTP 头，以便它们能够向远程服务器进行身份验证。Altair 允许你通过左侧的 Set Headers 菜单项配置自定义头。在没有身份验证 HTTP 头的情况下，查询可能会因 401 Unauthorized 等错误而失败。在第 7 章中，我们将探索启用了身份验证和授权控制的 GraphQL 服务器。

## 设置易受攻击的 GraphQL 服务器

现在我们已经有了查询任何 GraphQL 服务器所需的客户端工具，下一步是安装一个易受攻击的 GraphQL 服务器，我们将在整本书中将其用作目标。我们将在第 3 章中对 GraphQL 进行更深入的探索，以及第 4 章至第 9 章的渗透测试练习中使用此易受攻击的服务器。

## 安装 Docker

Docker (https://www.docker.com) 是一个用于部署和管理容器的工具。容器是打包代码及其依赖项的软件单元，以便应用程序可以在各种环境中可靠运行。Docker 可在 Windows、macOS 和 Linux 上使用。

我们将使用 Docker 部署本书中我们将攻击的应用程序。让我们从 Kali 软件仓库安装它，运行以下命令：

```
# sudo apt install -y docker.io
```

接下来，我们希望确保 Docker 进程在系统重新启动时自动启动：

```
# sudo systemctl enable docker --now
```

最后，确保 Docker 已成功安装：

```
# sudo docker

Management Commands:
  builder      Manage builds
  completion   generate the autocompletion script for the specified shell
  config       Manage Docker configs
  container    Manage containers
  context      Manage contexts
```
