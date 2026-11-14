

![](image/9781838821135cov_Low_Res.png)

# Advanced C++

掌握自信编写健壮 C++ 代码的技术

Gazihan Alankus

Olena Lizina

Rakesh Mane

Vivek Nagarajan

Brian Price

Copyright © 2019 Packt Publishing

版权所有。未经出版商事先书面许可，本书的任何部分不得以任何形式或任何方式复制、存储于检索系统或传播，除非是在评论文章或书评中引用简短片段。

本书在编写过程中已尽最大努力确保所呈现信息的准确性。然而，本书包含的信息不提供任何明示或暗示的担保。作者、Packt Publishing 及其经销商和分销商不对本书直接或间接导致的任何损害负责。

Packt Publishing 已尽力通过适当使用大写字母，提供书中提及的所有公司和产品的商标信息。然而，Packt Publishing 无法保证该信息的准确性。

作者：Gazihan Alankus、Olena Lizina、Rakesh Mane、Vivek Nagarajan 和 Brian Price

技术审阅员：Anil Achary 和 Deepak Selvakumar

项目编辑：Bhavesh Bangera

采购编辑：Kunal Sawant、Koushik Sen 和 Sneha Shinde

制作编辑：Samita Warang

编辑委员会：Shubhopriya Banerjee、Bharat Botle、Ewan Buckingham、Mahesh Dhyani、Manasa Kumar、Alex Mazonowicz、Bridget Neale、Dominic Pereira、Shiny Poojary、Abhisekh Rane、Erol Staveley、Ankita Thakur、Nitesh Thakur 和 Jonathan Wray。

首次出版：2019 年 10 月

制作参考：1311019

ISBN: 978-1-83882-113-5

由 Packt Publishing Ltd. 出版

Livery Place, 35 Livery Street

Birmingham B3 2PB, UK

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

