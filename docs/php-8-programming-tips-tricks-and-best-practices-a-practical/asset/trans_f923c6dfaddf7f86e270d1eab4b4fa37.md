
## 下载示例代码文件

你可以从 GitHub 下载本书的示例代码文件，地址为 https://github.com/PacktPublishing/PHP-8-Programming-Tips-Tricks-and-Best-Practices。如果代码有更新，将会在 GitHub 仓库中更新。

我们还有来自丰富图书和视频目录的其他代码包，可在 https://github.com/PacktPublishing/ 获取。快来看看吧！

## 下载彩色图片

我们还提供了一份 PDF 文件，其中包含本书中使用的屏幕截图和图表的彩色图片。你可以在此处下载：https://static.packt-cdn.com/downloads/9781801071871_ColorImages.pdf。

## 使用的约定

全书使用了多种文本约定。

Code in text: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: "This chapter also taught you how the new Attribute class can be used as an eventual replacement for PHP DocBlocks."

代码块设置如下：

```
```php
// /repo/ch01/php7_prop_reduce.php
declare(strict_types=1);
class Test {
    protected $id = 0;
    protected $token = 0;
    protected $name = '';
```
```

当我们希望引起您对代码块中特定部分的注意时，相关行或项目会以粗体显示：

```
```
$result = match(<EXPRESSION>) {
    <ITEM> => <EXPRESSION>,
    [<ITEM> => <EXPRESSION>,]
    default => <DEFAULT EXPRESSION>
};
```
```

任何命令行输入或输出均按如下方式书写：

```
```
Fatal error: Uncaught TypeError: Cannot assign string to property Test::$token of type int in /repo/ch01/php8_prop_danger.php:12
```
```

> **提示或重要说明**
> 显示如下。

## 联系我们

我们始终欢迎读者的反馈。

**一般反馈：** 如果您对本书的任何方面有疑问，请发送电子邮件至 customercare@packtpub.com，并在邮件主题中提及书名。

**勘误：** 虽然我们已尽最大努力确保内容的准确性，但错误难免。如果您在本書中发现错误，我们将不胜感激您向我们报告。请访问 www.packtpub.com/support/errata 并填写表格。

**盗版：** 如果您在互联网上发现我们作品的任何形式的非法复制，我们将不胜感激您提供位置地址或网站名称。请通过 copyright@packt.com 联系我们，并附上材料的链接。

**如果您有兴趣成为作者：** 如果您在某个主题上有专长，并且有兴趣撰写或为一本书做出贡献，请访问 authors.packtpub.com。

## 分享您的想法

在您阅读完 *PHP 8 Programming Tips, Tricks and Best Practices* 后，我们很想听听您的想法！请点击此处直接前往本书在亚马逊的评论页面并分享您的反馈。

您的评论对我们和技术社区都很重要，将帮助我们确保提供优质内容。

## 第 1 部分：

PHP 8 技巧

本部分介绍从未见过的酷炫内容，均是 PHP 8 的新特性。章节讨论新功能，首先在面向对象编程方面，其次是函数式和扩展级别的新内容。本部分最后一章涵盖直接的 C 语言原型设计。

本部分包含以下章节：

- 第 1 章，介绍新的 PHP 8 OOP 功能
- 第 2 章，了解 PHP 8 的函数式新增功能
- 第 3 章，利用错误处理增强功能
- 第 4 章，进行直接的 C 语言调用

## 1

### 介绍新的 PHP 8 OOP 功能

在本章中，您将了解特定于 **面向对象编程 (OOP)** 的新的 **PHP: Hypertext Preprocessor 8 (PHP 8)** 功能。本章包含一组可用于生成 CAPTCHA 图像的类（**CAPTCHA** 是 **Completely Automated Public Turing test to tell Computers and Humans Apart** 的缩写），清晰地展示了新的 PHP 8 功能和概念。本章对于帮助您快速将新的 PHP 8 功能融入自己的实践至关重要。这样一来，您的代码将运行得更快、更高效，bug 更少。

本章涵盖以下主题：

- 使用构造函数属性提升
- 使用属性
- 将 match 表达式纳入您的程序代码
- 理解命名参数
- 探索新的数据类型
- 使用类型化属性改进代码

## 技术要求

要检查并运行本章提供的代码示例，推荐的最低硬件配置如下：

- 基于 x86_64 的台式电脑或笔记本电脑
- 1 千兆字节 (**GB**) 可用磁盘空间
- 4 GB 的 **随机存取存储器** (**RAM**)
- 500 **每秒千比特** (**Kbps**) 或更快的互联网连接

此外，您需要安装以下软件：

- Docker
- Docker Compose

本书使用预构建的 Docker 镜像，其中包含创建和运行本书中涵盖的 PHP 8 代码示例所需的所有软件。您无需在计算机上安装 PHP、Apache 或 MySQL：只需使用 Docker 和提供的镜像即可。

要设置运行代码示例的测试环境，请按以下步骤操作：

1. 安装 Docker。
   如果您使用的是 Windows，请从此处开始：
   `https://docs.docker.com/docker-for-windows/install/`
   如果您使用的是 Mac，请从此处开始：
   `https://docs.docker.com/docker-for-mac/install/`
   如果您使用的是 Linux，请查看此处：
   `https://docs.docker.com/engine/install/`
2. 安装 Docker Compose。所有操作系统均从此处开始：
   `https://docs.docker.com/compose/install/`
3. 将与本书关联的源代码安装到您的本地计算机。
   如果您已安装 Git，请使用以下命令：

```
git clone https://github.com/PacktPublishing/PHP-8-Programming-Tips-Tricks-and-Best-Practices.git ~/repo
```

否则，您可以从此统一资源定位符 (URL) 下载源代码：https://github.com/PacktPublishing/PHP-8-Programming-Tips-Tricks-and-Best-Practices/archive/main.zip。然后您可以解压到您创建的文件夹中，我们在本书中将其称为 /repo。

4. 现在您可以启动 Docker 守护进程。对于 Windows 或 Mac，您只需激活 Docker Desktop 应用即可。

如果您使用的是 Ubuntu 或 Debian Linux，请执行以下命令：

```
sudo service docker start
```

对于 Red Hat、Fedora 或 CentOS，请使用以下命令：

```
sudo systemctl start docker
```

5. 构建与本书关联的 Docker 容器并使其上线。操作如下。

从您的本地计算机打开命令提示符（终端窗口）。将目录更改为 /repo。仅首次执行时，发出 docker-compose build 命令以构建环境。请注意，您可能需要 root（管理员）权限来运行 Docker 命令。如果是这种情况，请以管理员身份运行（适用于 Windows）或在命令前加上 sudo。根据您的连接速度，初始构建可能需要相当长的时间才能完成！

6. 要启动容器，请按以下步骤操作

7. 从您的本地计算机打开命令提示符（终端窗口）。将目录更改为 /repo。通过运行以下命令将 Docker 容器在后台模式下上线：

```
docker-compose up -d
```

请注意，您实际上不需要单独构建容器。如果在发出 docker-compose up 命令时容器尚未构建，它将被自动构建。另一方面，单独构建容器可能更方便，在这种情况下 docker build 就足够了。

这是一个有用的命令，可确保所有容器都在运行：

```
docker-compose ps
```

8. 要访问正在运行的 Docker 容器 Web 服务器，请按以下步骤操作。

在本地计算机上打开浏览器。输入此 URL 以访问 PHP 8 代码：

```
http://localhost:8888
```

输入此 URL 以访问 PHP 7 代码：

```
http://localhost:7777
```

9. 要打开到正在运行的 Docker 容器的命令 shell，请按以下步骤操作。

从您的本地计算机打开命令提示符（终端窗口）。执行此命令以访问 PHP 8 容器：

```
docker exec -it php8_tips_php8 /bin/bash
```

执行此命令以访问 PHP 7 容器：

```
docker exec -it php8_tips_php7 /bin/bash
```

10. 当您完成与容器的操作后，要将其下线，请从本地计算机打开命令提示符（终端窗口）并执行以下命令：

```
docker-compose down
```

本章的源代码位于此处：

```
https://github.com/PacktPublishing/PHP-8-Programming-Tips-Tricks-and-Best-Practices
```

> **重要说明**
> 如果您的主机计算机使用 **Advanced RISC Machines (ARM)** 架构（例如 Raspberry Pi），您将需要使用修改后的 Dockerfile。

> **提示**
> 通过阅读本文快速了解 Docker 技术和术语将是一个绝佳的主意：https://docs.docker.com/get-started/。

我们现在可以开始讨论，首先了解一下构造函数属性提升。
