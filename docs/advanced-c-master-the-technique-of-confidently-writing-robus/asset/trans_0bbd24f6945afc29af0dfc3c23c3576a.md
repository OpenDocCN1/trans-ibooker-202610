

## 安装 CMake

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

安装 Eclipse CDT 有多种方法。为了获得最新的稳定版本，我们将使用官方安装程序。访问此网站并下载 Linux 安装程序：[https://www.eclipse.org/downloads/packages/installer](https://www.eclipse.org/downloads/packages/installer)。

按照那里的说明安装 `Eclipse IDE for C/C++ Developers`。安装完成后，运行 Eclipse 可执行文件。如果您未更改默认配置，在终端中输入以下命令将运行它：

```
~/eclipse/cpp-2019-03/eclipse/eclipse
```

您将选择一个工作区文件夹，然后将在主 Eclipse 窗口中看到一个 `Welcome` 选项卡。

现在，我们将安装 `cmake4eclipse`。一种简单的方法是访问此网站，将 `Install` 图标拖动到 Eclipse 窗口中：[https://github.com/15knots/cmake4eclipse#installation](https://github.com/15knots/cmake4eclipse#installation)。它将要求您重启 Eclipse，之后您就可以修改 CMake 项目以使其在 Eclipse 中工作了。

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

本书的代码包也托管在 GitHub 上：[https://github.com/TrainingByPackt/Advanced-CPlusPlus](https://github.com/TrainingByPackt/Advanced-CPlusPlus)。

我们还在 [https://github.com/PacktPublishing/](https://github.com/PacktPublishing/) 上提供了来自我们丰富的图书和视频目录的其他代码包。去看看吧！

# 1. 可移植 C++ 软件的剖析

## 学习目标

在本章结束时，您将能够：

*   建立代码-构建-测试流程
*   描述编译的各个阶段
*   解读复杂的 C++ 类型系统
*   配置带有单元测试的项目
*   将源代码转换为目标代码
*   编写可读的代码并对其进行调试

在本章中，我们将学习建立贯穿全书的代码-构建-测试模型，编写优雅的代码，并执行单元测试。

## 简介

C++ 是您可以用来编写高效代码的最古老、最流行的语言之一。它既像 C 语言一样“贴近硬件”，又像 Java 一样拥有先进的面向对象特性。作为一种高效的低级语言，C++ 成为效率至关重要的领域（如游戏、模拟和嵌入式系统）的首选语言。同时，作为一种拥有泛型、引用以及其他无数先进特性的面向对象语言，它适合由多人开发和维护的大型项目。

几乎任何编程经验都涉及组织代码库和使用他人编写的库。C++ 也不例外。除非您的程序很简单，否则您需要将代码分发到多个需要组织的文件中，并使用各种库来完成任务，这些库通常比您自己的代码以更高效、更健壮的方式完成任务。不使用任何第三方库的 C++ 项目是边缘情况，并不代表大多数使用许多库的项目。这些项目及其库预期要在不同的硬件架构和操作系统上工作。因此，如果您打算用 C++ 开发任何有意义的东西，花时间进行项目设置并了解用于管理依赖关系的工具非常重要。

大多数现代流行的高级语言都有用于维护项目、构建项目和处理库依赖关系的标准工具。其中许多拥有托管库的存储库，以及可自动从这些存储库下载并使用库的工具。例如，Python 有 `pip`，它负责下载和使用程序员想要使用的库的适当版本。同样，JavaScript 有 `npm`，Java 有 `maven`，Dart 有 `pub`，C# 有 `NuGet`。在大多数这些语言中，您列出想要使用的库的名称和版本，工具会自动下载并使用兼容版本的库。这些语言受益于程序在受控环境中构建和运行的事实，该环境满足一定的硬件和软件要求。另一方面，C++ 预期要在各种上下文中工作，包括非常原始的硬件在内的不同架构。因此，在构建程序和执行依赖管理方面，C++ 程序员享受的“照顾”较少。

## 管理 C++ 项目

在 C++ 的世界里，我们有几个工具可以帮助管理项目源代码及其依赖关系。例如，`pkg-config`、`Autotools`、`make` 和 `CMake` 是社区中最著名的工具。与其他高级语言的工具相比，这些工具的使用要复杂得多。在这些工具中，`CMake` 已成为管理 C++ 项目及其依赖关系的事实标准。与 `make` 相比，它更具主见性，并被大多数 IDE（集成开发环境）接受为直接的项目格式。

