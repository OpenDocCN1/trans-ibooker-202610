# 前言

**关于本节**

本节简要介绍作者、本书涵盖的内容、你开始学习所需的技术技能，以及完成所有活动和练习所需的硬件和软件要求。

## 关于本书

C++ 是使用最广泛的编程语言之一，应用于从游戏到**图形用户界面（GUI）**编程甚至操作系统的各个领域。如果你希望拓展职业机会，掌握 C++ 的高级特性是关键。

本书从高级 C++ 概念开始，帮助你解读复杂的 C++ 类型系统，并理解编译的各个阶段如何将源代码转换为目标代码。然后，你将学习如何识别用于控制执行流程、捕获数据和传递数据的工具。通过创建小型模型，你甚至会发现如何在 C++ 中使用高级 lambda 和捕获，并表达常见的 API 设计模式。在后续章节中，你将通过学习内存对齐、缓存访问以及程序运行时间来探索优化代码的方法。最后一章将通过理解现代 CPU 分支预测以及如何让你的代码对缓存友好，帮助你最大化性能。

读完本书后，你将掌握令你从其他 C++ 程序员中脱颖而出的编程技能。

## 关于作者

**Gazihan Alankus** 拥有圣路易斯华盛顿大学计算机科学博士学位。目前，他是土耳其伊兹密尔经济大学的助理教授。他教授并研究游戏开发、移动应用开发和人机交互。他是 Google Dart 开发者专家，并在其于 2019 年创立的公司 Gbot 中与学生一起开发 Flutter 应用。

**Olena Lizina** 是一名拥有 5 年 C++ 经验的软件开发者。她在一家国际产品公司拥有开发远程计算机监控和管理系统的实践经验，该系统服务于大量用户。在过去的 4 年里，她一直在国际外包公司从事知名汽车企业的汽车项目。她参与了多个项目中复杂且高性能应用的开发，例如**HMI（人机界面）**、导航以及与传感器相关的应用。

**Rakesh Mane** 拥有超过 18 年的软件行业经验。他曾与来自印度、美国和新加坡等地的熟练程序员合作。他主要从事 C++、Python、shell 脚本和数据库方面的工作。闲暇时，他喜欢听音乐和旅行。此外，他还喜欢使用软件工具和代码进行尝试、实验和破坏。

**Vivek Nagarajan** 是一名自学成才的程序员，于 20 世纪 80 年代从 8 位系统起步。他参与过大量软件项目，拥有 14 年的 C++ 专业经验。除此之外，多年来他还使用过多种语言和框架。他是一名业余力量举选手、DIY 爱好者和摩托车赛车手。目前，他是一名独立软件顾问。

**Brian Price** 拥有超过 30 年的多种语言、项目和行业工作经验，其中包括超过 20 年的 C++ 经验。他曾经从事发电站模拟器、SCADA 系统和医疗设备方面的工作。目前，他正使用 C++、CMake 和 Python 为下一代医疗设备开发软件。他喜欢用多种语言解决谜题和 Euler 项目。

## 学习目标

通过本书，你将能够：

- 深入探究 C++ 的结构和工作流程
- 研究 C++ 中不同编码方法的优缺点
- 测试、运行和调试你的程序
- 将目标文件链接为动态库
- 使用模板、SFINAE、constexpr if 表达式和可变参数模板
- 在资源管理中应用最佳实践

## 目标读者

如果你使用过 C++，但希望学习如何充分利用这门语言，尤其是在大型项目中，那么本书适合你。你需要对编程有基本的了解，并且知道如何在项目目录中使用编辑器生成代码文件。同时，建议你具备一些强类型语言（如 C 和 C++）的使用经验。

## 方法

这本节奏紧凑的书籍旨在通过描述性图形和具有挑战性的练习，帮助你快速掌握概念。书中将包含“标注”，提供关键要点和最常见的陷阱，以保持你的兴趣，同时将主题划分为易于管理的小节。

## 硬件要求

为了获得最佳学习体验，我们推荐以下硬件配置：

- 任何入门级 PC/Mac，运行 Windows、Linux 或 macOS 均可
- 处理器：双核或同等性能
- 内存：4 GB RAM（推荐 8 GB）
- 存储：35 GB 可用空间

## 软件要求

你还需要预先安装以下软件：

- 操作系统：Windows 7 SP1 32/64 位、Windows 8.1 32/64 位或 Windows 10 32/64 位、Ubuntu 14.04 或更高版本，或 macOS Sierra 或更高版本
- 浏览器：Google Chrome 或 Mozilla Firefox

## 安装与设置

在开始学习本书之前，你需要安装本书中使用的以下库。你将在这里找到安装步骤。

### 安装 CMake

我们将使用 CMake 3.12.1 或更高版本。我们有两种安装选项。

选项 1：

如果你使用的是 Ubuntu 18.10，可以通过以下命令全局安装 CMake：

```
sudo apt install cmake
```

当你运行以下命令时：

```
cmake -version
```

你应该看到以下输出：

```
cmake version 3.12.1

CMake suite maintained and supported by Kitware (kitware.com/cmake).
```

如果你看到的版本低于 3.12.1（例如 3.10），则应按照以下说明在本地安装 CMake。

选项 2：



如果您使用的是较旧的 Linux 版本，您可能会得到低于 3.12.1 的 CMake 版本。然后，您需要本地安装它。使用以下命令：

```
wget \
https://github.com/Kitware/CMake/releases/download/v3.15.1/cmake-3.15.1-Linux-x86_64.sh

sh cmake-3.15.1-Linux-x86_64.sh
```

当您看到软件许可协议时，输入 y 并按 Enter。当被询问安装位置时，再次输入 y 并按 Enter。这应该会将其安装到系统中的一个新文件夹。

现在，我们将把该文件夹添加到我们的路径中。输入以下内容。请注意，第一行有点太长且在此文档中换行。您应该将其作为单行写入，如下所示：

```
echo "export PATH=\"$HOME/cmake-3.15.1-Linux-x86_64/bin:$PATH\"" >> .bash_profile

source .profile
```

现在，当您输入以下内容时：

```
cmake -version
```

您应该会看到以下输出：

```
cmake version 3.15.1

CMake suite maintained and supported by Kitware (kitware.com/cmake).
```

3.15.1 是撰写本文档时当前的最新版本。由于它比 3.12.1 更新，因此足以满足我们的目的。

### 安装 Git

通过输入以下内容测试当前安装：

```
git --version
```

您应该会看到类似以下的行：

```
git version 2.17.1
```

如果您看到以下行，则需要安装 git：

```
command 'git' not found
```

以下是如何在 Ubuntu 中安装 **git** 的方法：

```
sudo apt install git
```

### 安装 g++

通过输入以下内容测试当前安装：

```
g++ --version
```

您应该会看到类似以下输出：

```
g++ (Ubuntu 7.4.0-1ubuntu1~18.04) 7.4.0

Copyright (C) 2017 Free Software Foundation, Inc.

This is free software; see the source for copying conditions. There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

如果未安装，输入以下代码进行安装：

```
sudo apt install g++
```

### 安装 Ninja

通过输入以下内容测试当前安装：

```
ninja --version
```

您应该会看到类似以下输出：

```
1.8.2
```

如果未安装，输入以下代码进行安装：

```
sudo apt install ninja-build
```

### 安装 Eclipse CDT 和 cmake4eclipse

安装 Eclipse CDT 有多种方法。为了获取最新稳定版本，我们将使用官方安装程序。前往此网站并下载 Linux 安装程序：https://www.eclipse.org/downloads/packages/installer。

按照那里的说明进行操作并安装 **Eclipse IDE for C/C++ Developers**。安装完成后，运行 Eclipse 可执行文件。如果您未更改默认配置，在终端中输入以下命令即可运行它：

```
~/eclipse/cpp-2019-03/eclipse/eclipse
```

您将选择一个工作区文件夹，然后您将在主 Eclipse 窗口中看到一个 **Welcome** 标签页。

现在，我们将安装 **cmake4eclipse**。一种简单的方法是前往此网站并将 **Install** 图标拖到 Eclipse 窗口：[https://github.com/15knots/cmake4eclipse#installation](https://github.com/15knots/cmake4eclipse#installation)。它会要求您重新启动 Eclipse，之后您就可以修改 CMake 项目以在 Eclipse 中使用。

### 安装 GoogleTest

我们将在系统中安装 **GoogleTest**，这也会安装依赖于它的其他软件包。输入以下命令：

```
sudo apt install libgtest-dev google-mock
```

此命令安装 **GoogleTest** 的头文件和源文件。现在，我们需要构建我们安装的源文件以创建 **GoogleTest** 库。运行以下命令来执行此操作：

```
cd /usr/src/gtest
sudo cmake CMakeLists.txt
sudo make
sudo cp *.a /usr/lib
```

### 安装代码包

将课程的代码包复制到 `C:/Code` 文件夹。

## 附加资源

本书的代码包也托管在 GitHub 上，地址为 [https://github.com/TrainingByPackt/Advanced-CPlusPlus](https://github.com/TrainingByPackt/Advanced-CPlusPlus)。

我们还有来自我们丰富的图书和视频目录的其他代码包，可在 [https://github.com/PacktPublishing/](https://github.com/PacktPublishing/) 获取。快来看看吧！

## 引言

C++ 是最古老且最流行的语言之一，可用于编写高效代码。它既“接近金属”，像 C 一样，又具有像 Java 一样的高级面向对象特性。作为一种高效的低级语言，C++ 成为游戏、模拟和嵌入式系统等效率至关重要的领域的首选语言。同时，作为一种具有泛型、引用等高级特性的面向对象语言，它也适用于由多人开发和维护的大型项目。

几乎任何编程经验都涉及组织代码库和使用他人编写的库。C++ 也不例外。除非您的程序很简单，否则您需要将代码分布到多个文件中进行组织，并使用各种库来完成任务，通常比您自己的代码更高效、更健壮。不使用任何第三方库的 C++ 项目是边缘情况，并不代表大多数使用大量库的项目。这些项目及其库需要在不同的硬件架构和操作系统上运行。因此，如果您要用 C++ 开发任何有意义的东西，投入时间进行项目设置并了解用于管理依赖项的工具是很重要的。

大多数现代流行的高级语言都有标准的工具来维护项目、构建它们并处理其库依赖项。其中许多都有托管库和工具的存储库，可自动从这些存储库下载并使用库。例如，Python 有 **pip**，它负责下载和使用程序员想要使用的库的适当版本。类似地，JavaScript 有 **npm**，Java 有 **maven**，Dart 有 **pub**，C# 有 **NuGet**。在大多数这些语言中，您列出想要使用的库的名称和版本，工具就会自动下载并使用库的兼容版本。这些语言受益于程序在受控环境中构建和运行的事实，其中满足了一定水平的硬件和软件要求。另一方面，C++ 预计能在各种环境中工作，具有不同的架构，包括非常原始的硬件。因此，与构建程序和执行依赖管理相比，C++ 程序员得到的“宠爱”较少。
