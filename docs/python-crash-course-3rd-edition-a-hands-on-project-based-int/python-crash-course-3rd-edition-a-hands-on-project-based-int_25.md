## 附录 E：部署故障排除

![](img/8f15563c16d2a98c4d3115150679b092_782_0.png)

当部署应用程序成功时，尤其是第一次部署时，会带来极大的满足感。然而，部署过程中可能出现许多障碍，而且不幸的是，其中一些问题可能难以识别和解决。本附录将帮助你了解现代部署方法，并在部署过程出现问题时提供具体的故障排除方法。

如果本附录中的额外信息不足以帮助你成功完成部署过程，请参阅 [https://ehmatthes.github.io/pcc_3e](https://ehmatthes.github.io/pcc_3e) 上的在线资源；那里的更新几乎肯定会帮助你成功部署。

### 了解部署

当你尝试排除特定部署尝试的问题时，了解典型部署的工作原理会很有帮助。部署是指将在本地系统上运行的项目复制到远程服务器上的过程，以便它能够响应互联网上任何用户的请求。远程环境与典型的本地系统在许多重要方面有所不同：它可能不是你正在使用的相同操作系统（OS），并且它很可能是一个物理服务器上的众多虚拟服务器之一。

当你部署项目或将其推送到远程服务器时，需要执行以下步骤：

- 在数据中心的物理机器上创建一个虚拟服务器。
- 在本地系统和远程服务器之间建立连接。
- 将项目的代码复制到远程服务器。
- 识别项目的所有依赖项并在远程服务器上安装它们。
- 设置数据库并运行任何现有迁移。
- 将静态文件（CSS、JavaScript 文件和媒体文件）复制到可以高效提供服务的位置。
- 启动服务器以处理传入请求。
- 项目准备好处理请求后，开始将传入请求路由到项目。





在初始化 Git 仓库之前先创建 Platform.sh 项目，然后尝试推送项目：

```
$ platform push
① Enter a number to choose a project:
  [0] ll_project (votohz445ljyg)
  > 0

②   [RootNotFoundException]
    Project root not found. This can only be run from inside a
    project directory.

③   To set the project for this Git repository, run:
      platform project:set-remote [id]
```

我们正在尝试推送一个项目，但本地项目尚未与远程项目关联。因此，Platform.sh CLI 会询问我们想要推送到哪个远程项目 ①。我们输入 0，选择了唯一列出的项目。但接下来，我们看到了 RootNotFoundException ②。这是因为 Platform.sh 在检查本地项目时，会查找 `.git` 目录，以确定如何将本地项目与远程项目连接起来。在本例中，由于在创建远程项目时不存在 `.git` 目录，因此从未建立该连接。CLI 建议了一个修复方法 ③；它告诉我们可以使用 `project:set-remote` 命令指定应与此本地项目关联的远程项目。

让我们尝试这个建议：

```
$ platform project:set-remote votohz445ljyg
Setting the remote project for this repository to: ll_project (votohz445ljyg)

The remote project for this repository is
  now set to: ll_project (votohz445ljyg)
```

在之前的输出中，CLI 显示了此远程项目的 ID，即 votohz445ljyg。因此我们使用此 ID 运行建议的命令，CLI 就能够在本地项目和远程项目之间建立连接。

现在让我们再次尝试推送项目：

```
$ platform push
Are you sure you want to push to the main (production) branch? [Y/n] y
Pushing HEAD to the existing environment main
--snip--
```

这是一次成功的推送；按照屏幕上的建议操作是有效的。

你应该谨慎运行自己不完全理解的命令。然而，如果你有充分理由相信某个命令造成的危害很小，并且你信任建议的来源，那么尝试工具提供的建议可能是合理的。

> **NOTE**
> 
> *请记住，有人会告诉你运行会抹掉你的系统或使系统暴露于远程攻击的命令。遵循你信任的公司或组织提供的工具的建议，与遵循网上随机人士的建议是不同的。只要涉及远程连接，请始终保持高度谨慎。*

## 阅读日志输出

如前所述，当你运行像 `platform push` 这样的命令时所看到的日志输出既信息丰富又令人望而生畏。阅读以下来自另一次使用 `platform push` 的尝试的日志输出片段，看看你是否能发现问题：

```
--snip--
Collecting soupsieve==2.3.2.post1
  Using cached soupsieve-2.3.2.post1-py3-none-any.whl (37 kB)
Collecting sqlparse==0.4.2
  Using cached sqlparse-0.4.2-py3-none-any.whl (42 kB)
Installing collected packages: platformshconfig, sqlparse,...
Successfully installed Django-4.1 asgiref-3.5.2 beautifulsoup4-4.11.1...
W: ERROR: Could not find a version that satisfies the requirement gunicorn
W: ERROR: No matching distribution found for gunicorn

130 static files copied to '/app/static'.

Executing pre-flight checks...
--snip--
```

当部署尝试失败时，一个好的策略是查看日志输出，看看是否能发现看起来像警告或错误的内容。警告相当常见；它们通常是关于项目依赖项即将发生变化的消息，以帮助开发者在问题导致实际失败之前解决它们。

一次成功的推送可能包含警告，但不应包含任何错误。在本例中，Platform.sh 无法找到安装 requirement gunicorn 的方法。这是 `requirements_remote.txt` 文件中的一个拼写错误，本应包含 gunicorn（多一个 r）。并非总是能轻易在日志输出中发现根本问题，尤其是在问题导致大量连锁错误和警告时。就像在本地系统上阅读回溯时一样，最好仔细查看列出的前几个错误和最后几个错误。中间的大多数错误往往是内部包抱怨出了问题，并将错误信息传递给其他内部包。我们可以修复的实际错误通常是列出的第一个或最后一个错误。

有时，你能够发现错误，有时，你完全不知道输出是什么意思。尝试一下绝对值得，而利用日志输出成功诊断错误是一种极其令人满意的感觉。随着你花费更多时间查看日志输出，你会变得更擅长识别对你最有意义的信息。

## 操作系统特定故障排除

你可以在任何你喜欢的操作系统上进行开发，并推送到任何你喜欢的主机。用于推送项目的工具已经发展到足以根据需要修改你的项目，使其在远程系统上正确运行。然而，可能会出现一些特定于操作系统的 issues。

在 Platform.sh 部署过程中，最可能出现困难的来源之一是安装 CLI。以下是执行此操作的命令：

```
$ curl -fsS https://platform.sh/cli/installer | php
```

该命令以 curl 开头，这是一个工具，可让你在终端中通过 URL 请求远程资源。在这里，它被用来从 Platform.sh 服务器下载 CLI 安装程序。命令中的 `-fsS` 部分是一组修改 curl 运行方式的标志。`f` 标志告诉 curl 抑制大多数错误消息，以便 CLI 安装程序可以处理它们，而不是全部报告给你。`s` 标志告诉 curl 静默运行；它让 CLI 安装程序决定在终端中显示哪些信息。`S` 标志告诉 curl 如果整个命令失败则显示错误消息。

与 Linux 为基础的远程服务器所使用的相同。基础 Windows 系统拥有一组与基础 Linux 系统不同的工具和语言，因此要在 Windows 上进行部署工作，你需要选择如何将基于 Linux 的工具集集成到本地环境中。

### Windows Subsystem for Linux

一种流行的方法是使用 *Windows Subsystem for Linux (WSL)*，这是一个允许 Linux 直接在 Windows 上运行的环境。如果你已设置 WSL，在 Windows 上使用 Platform.sh CLI 就像在 Linux 上使用一样简单。CLI 不会知道它运行在 Windows 上；它只会看到你使用它的 Linux 环境。

设置 WSL 是一个两步过程：你首先安装 WSL，然后选择要安装到 WSL 环境中的 Linux 发行版。设置 WSL 环境超出了本文的描述范围；如果你对这种方法感兴趣且尚未设置，请参阅 https://docs.microsoft.com/en-us/windows/wsl/about 的文档。一旦你设置好 WSL，你可以按照本附录 Linux 部分中的说明继续你的部署工作。

### Git Bash

构建可用于部署的本地环境的另一种方法使用 *Git Bash*，这是一个与 Bash 兼容但在 Windows 上运行的终端环境。Git Bash 在你使用 https://git-scm.com 上的安装程序安装 Git 时会一并安装。这种方法可行，但不如 WSL 流畅。在这种方法中，你需要对某些步骤使用 Windows 终端，对其他步骤使用 Git Bash 终端。

首先你需要安装 PHP。你可以使用 *XAMPP* 来完成，这是一个将 PHP 与一些其他面向开发者的工具捆绑在一起的包。前往 https://apachefriends.org 并点击下载 Windows 版 XAMPP 的按钮。打开安装程序并运行它；如果看到关于用户账户控制 (UAC) 限制的警告，点击 **OK**。接受安装程序的所有默认值。

安装程序运行完毕后，你需要将 PHP 添加到系统的路径中；这将告诉 Windows 在你想要运行 PHP 时去哪里查找。在开始菜单中输入 path 并点击 **编辑系统环境变量**；点击标有 **环境变量** 的按钮。你应该看到变量 Path 被高亮显示；在此窗格下点击 **编辑**。点击 **新建** 以向当前路径列表添加新路径。假设你在运行 XAMPP 安装程序时保留了默认设置，请在出现的框中添加 `c:\xampp\php`，然后点击 **OK**。完成后，关闭所有仍打开的系统对话框。

在满足这些要求后，你可以安装 Platform.sh CLI。你需要使用具有管理员权限的 Windows 终端；将 command 输入到开始菜单中，并在命令提示符应用下点击 **以管理员身份运行**。在出现的终端中，输入以下命令：

```
> curl -fsS https://platform.sh/cli/installer | php
```

这将安装 Platform.sh CLI，如前所述。

最后，你将在 Git Bash 中工作。要打开 Git Bash 终端，请转到开始菜单并搜索 git bash。点击出现的 **Git Bash 应用**；你应该会看到一个终端窗口打开。你可以在此终端中使用传统的基于 Linux 的命令，如 ls，以及基于 Windows 的命令，如 dir。为确保安装成功，发出 `platform list` 命令。你应该会看到 Platform.sh CLI 中所有命令的列表。从这里


在全新的Ubuntu安装中，它会确切地告诉你需要哪些软件包：

```
$ curl -fsS https://platform.sh/cli/installer | php
Command 'curl' not found, but can be installed with:
sudo apt  install curl
Command 'php' not found, but can be installed with:
sudo apt install php-cli
```

实际输出将提供其他一些可用软件包的更多信息，以及一些版本信息。以下命令将安装curl和PHP：

```
$ sudo apt install curl php-cli
```

运行此命令后，Platform.sh CLI 安装命令应能成功执行。由于你的本地环境与大多数基于Linux的托管环境非常相似，你在终端中学习到的大部分内容也将适用于远程环境。

## 其他部署方式

如果Platform.sh不适合你，或者你想尝试不同的方式，有很多托管平台可供选择。有些平台的工作方式与第20章描述的流程类似，有些平台在执行本附录开头描述的步骤时则采用截然不同的方法：Platform.sh允许你使用浏览器来执行我们使用CLI完成的步骤。如果你更喜欢基于浏览器的界面，而不是基于终端的工作流，你可能会更喜欢这种方法。

还有许多其他托管提供商同时提供基于CLI和基于浏览器的方法。其中一些提供商在浏览器中提供终端，因此你无需在系统上安装任何东西。

一些提供商允许你将项目推送到GitHub等远程代码托管站点，然后将你的GitHub仓库连接到托管站点。然后主机会从GitHub拉取你的代码，而不是要求你从本地系统直接推送到主机。Platform.sh也支持这种工作流。

一些提供商提供一系列服务供你选择，以便为你的项目组合出合适的基础设施。这通常要求你对部署过程以及远程服务器为项目提供服务所需的条件有更深入的理解。这些主机包括亚马逊网络服务（AWS）和微软的Azure平台。在这类平台上跟踪成本可能要困难得多，因为每项服务都可能独立产生费用。

许多人将项目托管在虚拟专用服务器（VPS）上。在这种方式中，你租用一台虚拟服务器，它就像一台远程计算机，登录服务器，安装运行项目所需的软件，将代码复制过去，设置正确的连接，然后让服务器开始接受请求。

新的托管平台和方法不断涌现；找一个吸引你的平台，并花时间学习该提供商的部署过程。维护你的项目足够长的时间，以便了解提供商的方法中哪些有效、哪些无效。没有哪个托管平台是完美的；你需要持续判断当前使用的提供商是否足以满足你的用例。

关于选择部署平台和整体部署方法，我要最后提醒一句。有些人会热情地引导你采用过于复杂的部署方法和服务，这些方法和服务旨在让你的项目高度可靠，并能同时服务数百万用户。许多程序员花费大量时间、金钱和精力构建复杂的部署策略，结果却发现几乎没有人使用他们的项目。大多数Django项目都可以在小型托管方案上设置，并调整到每分钟处理数千个请求。如果你的项目流量低于这个水平，在投资用于世界上一些最大型网站的基础设施之前，花时间将部署配置为在最小化平台上良好运行。

部署有时极具挑战性，但当你的线上项目运行良好时，也同样令人满足。享受挑战，需要时寻求帮助。
