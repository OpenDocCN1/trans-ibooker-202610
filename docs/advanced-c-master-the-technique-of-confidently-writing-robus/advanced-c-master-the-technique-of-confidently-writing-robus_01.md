# 前言

## 关于

本节简要介绍作者、本书涵盖的内容、入门所需的技术技能，以及完成所有活动和练习所需的硬件和软件要求。

## 关于本书

C++ 是最广泛使用的编程语言之一，应用于各种领域，从游戏到**图形用户界面（GUI）**编程，甚至操作系统。如果您希望拓展职业机会，掌握 C++ 的高级特性至关重要。

本书从高级 C++ 概念入手，帮助您解读复杂的 C++ 类型系统，并理解编译的各个阶段如何将源代码转换为目标代码。随后，您将学习如何识别所需工具，以控制执行流程、捕获数据和传递数据。通过创建小型模型，您甚至将发现如何使用高级 Lambda 表达式和捕获，以及如何在 C++ 中表达常见的 API 设计模式。随着章节深入，您将探索优化代码的方法，学习内存对齐、缓存访问和程序运行时间。最后一章将帮助您通过理解现代 CPU 分支预测以及如何使代码对缓存友好来最大化性能。

读完本书后，您将开发出使您区别于其他 C++ 程序员的编程技能。

### 关于作者

**Gazihan Alankus** 拥有圣路易斯华盛顿大学计算机科学博士学位。目前，他是土耳其伊兹密尔经济大学的助理教授。他教授并研究游戏开发、移动应用开发和人机交互。他是 Dart 方面的 Google 开发者专家，并在 2019 年创立的公司 Gbot 中与学生一起开发 Flutter 应用。

**Olena Lizina** 是一拥有 5 年 C++ 经验的软件开发人员。她拥有为国际产品公司开发监控和管理远程计算机系统的实践知识，用户量大。过去 4 年，她一直在国际外包公司为知名汽车企业从事汽车项目。她参与了不同项目中复杂且高性能应用的开发，如**人机界面（HMI）**、导航和传感器应用。

**Rakesh Mane** 在软件行业拥有 18 年以上经验。他曾与来自不同地区的熟练程序员合作：印度、美国和新加坡。他主要使用 C++、Python、Shell 脚本和数据库。业余时间，他喜欢听音乐和旅行。此外，他喜欢使用软件工具和代码进行摆弄、实验和破坏事物。

**Vivek Nagarajan** 是一位自学成才的程序员，20 世纪 80 年代起在 8 位系统上起步。他参与过大量软件项目，拥有 14 年专业 C++ 经验。除此之外，多年来他还涉足各种语言和框架。他是业余举重爱好者、DIY 爱好者和摩托车赛车手。目前担任独立软件顾问。

**Brian Price** 拥有 30 多年经验，涉足多种语言、项目和行业，包括 20 多年 C++ 经验。他曾从事电站模拟器、SCADA 系统和医疗设备工作。目前正在用 C++、CMake 和 Python 为下一代医疗设备打造软件。他喜欢用多种语言解谜和做欧拉项目。

### 学习目标

读完本书后，您将能够：

*   深入探究 C++ 的解剖结构和工作流程
*   研究 C++ 不同编码方法的优缺点
*   测试、运行和调试程序
*   将目标文件链接为动态库
*   使用模板、SFINAE、`constexpr if` 表达式和可变参数模板
*   应用资源管理的最佳实践

### 受众

如果您曾使用 C++ 但想学习如何充分利用这门语言，尤其是用于大型项目，本书适合您。必须具备编程的一般理解，并懂得使用编辑器在项目目录中生成代码文件。还建议有 C 和 C++ 等强类型语言的经验。

### 方法

这本节奏快的书旨在通过描述性图形和具有挑战性的练习快速教授概念。书中将有"标注"，包含关键要点和最常见的陷阱，以保持您的兴趣，同时将主题分解为易于管理的部分。

### 硬件要求

为获得最佳学习体验，我们推荐以下硬件配置：

*   任何配备 Windows、Linux 或 macOS 的入门级 PC/Mac 均可
*   处理器：双核或同等级别
*   内存：4 GB RAM（建议 8 GB）
*   存储：35 GB 可用空间

### 软件要求

您还需要预先安装以下软件：

*   操作系统：Windows 7 SP1 32/64 位、Windows 8.1 32/64 位或 Windows 10 32/64 位，Ubuntu 14.04 或更高版本，或 macOS Sierra 或更高版本
*   浏览器：Google Chrome 或 Mozilla Firefox

### 安装与设置

开始阅读本书前，您需要安装本书使用的以下库。您可以在此找到安装步骤。

#### 安装 CMake

我们将使用 CMake 3.12.1 或更高版本。有两种安装选项。

**选项 1：**

如果您使用 Ubuntu 18.10，可以使用以下命令全局安装 CMake：

```
sudo apt install cmake
```

当您运行以下命令时：

```
cmake --version
```

您应该看到以下输出：

```
cmake version 3.12.1
CMake suite maintained and supported by Kitware (`kitware.com/cmake`)。
```

如果此处显示的版本低于 3.12.1（例如 3.10），您应使用以下说明在本地安装 CMake。

**选项 2：**





## 安装 CMake (continued)

如果您使用的是较旧的 Linux 版本，可能会获得低于 3.12.1 的 CMake 版本。此时，您需要在本地安装它。请使用以下命令：

```
wget https://github.com/Kitware/CMake/releases/download/v3.15.1/cmake-3.15.1-Linux-x86_64.sh
```

```
sh cmake-3.15.1-Linux-x86_64.sh
```

当您看到软件许可证时，输入 `y` 并按 `Enter` 键。当询问安装位置时，再次输入 `y` 并按 Enter 键。这应该会将其安装到系统中的一个新文件夹中。

现在，我们将把该文件夹添加到我们的路径中。输入以下内容。请注意，第一行有点太长，在本文档中换行了。您应该将其写在一行中，如下所示：

```
echo "export PATH=\"$HOME/cmake-3.15.1-Linux-x86_64/bin:$PATH\"" >> .bash_profile
```

```
source .profile
```

现在，当您输入以下内容时：

```
cmake –version
```

您应该会看到以下输出：

```
cmake version 3.15.1
CMake suite maintained and supported by Kitware (kitware.com/cmake).
```

在撰写本文档时，3.15.1 是当前的最新版本。由于它比 3.12.1 新，因此足以满足我们的需求。

## 安装 Git

通过输入以下内容测试当前安装：

```
git --version
```

您应该会看到如下所示的一行：

```
git version 2.17.1
```

如果您看到以下行，则需要安装 `git`：

```
command 'git' not found
```

以下是在 Ubuntu 中安装 `git` 的方法：

```
sudo apt install git
```

## 安装 g++

通过输入以下内容测试当前安装：

```
g++ --version
```

您应该会看到如下输出：

```
g++ (Ubuntu 7.4.0-1ubuntu1~18.04) 7.4.0
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions. There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

如果未安装，请输入以下代码进行安装：

```
sudo apt install g++
```

## 安装 Ninja

通过输入以下内容测试当前安装：

```
ninja --version
```

您应该会看到如下输出：

```
1.8.2
```

如果未安装，请输入以下代码进行安装：

```
sudo apt install ninja-build
```

## 安装 Eclipse CDT 和 cmake4eclipse

安装 Eclipse CDT 有多种方法。为了获得最新的稳定版本，我们将使用官方安装程序。访问此网站并下载 Linux 安装程序：[`www.eclipse.org/downloads/packages/installer`](https://www.eclipse.org/downloads/packages/installer)。

按照那里的说明安装 `Eclipse IDE for C/C++ Developers`。安装完成后，运行 Eclipse 可执行文件。如果您未更改默认配置，在终端中输入以下命令将运行它：

```
~/eclipse/cpp-2019-03/eclipse/eclipse
```

您将选择一个工作区文件夹，然后将在主 Eclipse 窗口中看到一个 `Welcome` 选项卡。

现在，我们将安装 `cmake4eclipse`。一种简单的方法是访问此网站，将 `Install` 图标拖动到 Eclipse 窗口中：[`github.com/15knots/cmake4eclipse#installation`](https://github.com/15knots/cmake4eclipse#installation)。它将要求您重启 Eclipse，之后您就可以修改 CMake 项目以使其在 Eclipse 中工作了。

## 安装 GoogleTest

我们将在系统中安装 `GoogleTest`，这也将安装其他依赖它的软件包。输入以下命令：

```
sudo apt install libgtest-dev google-mock
```

此命令安装 `GoogleTest` 的包含文件和源文件。现在，我们需要构建刚才安装的源文件以创建 `GoogleTest` 库。运行以下命令来执行此操作：

```
cd /usr/src/gtest
```

```
sudo cmake CMakeLists.txt
```

```
sudo make
```

```
sudo cp *.a /usr/lib
```

### 安装代码包

将课程的代码包复制到 `C:/Code` 文件夹。

### 附加资源

本书的代码包也托管在 GitHub 上：[`github.com/TrainingByPackt/Advanced-CPlusPlus`](https://github.com/TrainingByPackt/Advanced-CPlusPlus)。

我们还在 [`github.com/PacktPublishing/`](https://github.com/PacktPublishing/) 上提供了来自我们丰富的图书和视频目录的其他代码包。去看看吧！
