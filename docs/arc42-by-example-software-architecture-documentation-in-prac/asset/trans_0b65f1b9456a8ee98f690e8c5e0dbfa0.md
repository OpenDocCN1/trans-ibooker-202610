
以下章节描述了 docToolchain 的架构。整个项目及其剩余文档托管在 GitHub 上，网址为 [https://doctoolchain.github.io/docToolchain/](https://doctoolchain.github.io/docToolchain/)。

## VI.1 简介和目标

将 `arc42` 解决方案架构以不同格式交付的需求是 docToolchain 诞生的主要驱动力。作为一名技术人员，我看到了基于文本的文档的价值。然而，其他利益相关者希望文档：
*   *"其他所有文档所在之处"*—在 wiki 中
*   *"我们一直以来文档化的方式"*—使用 MS Word
*   *"以可修订安全格式"*—作为 PDF

不久，在满足这些需求后，我注意到更新变化的 UML 图表相当困难。“这不能自动化吗？”，我自问并创建了 `exportEA` 任务作为一个简单的 Visual Basic 脚本。

我并没有在绘图板上规划 docToolchain。相反，它以敏捷的方式演进；也就是说，通过用户的需求。

与 Dr. Gernot Starke 一起，我就这种方法做了演讲，并从观众那里收到了一些有趣的反馈，例如 *"…但我不使用 Enterprise Architect——我也可以使用 MS Visio 吗？"*

我的回答大致如下：
*"不，目前不行。但 docToolchain 不仅仅是一个工具。我希望它成为一种理念——一种思维方式的改变。如果 EA 可以轻松自动化，我相信 Visio 也可以被自动化！"*

我没有预料到的是，在我做出此陈述的几天后，我们收到了带有 `exportVisio` 任务的 pull request！

以这种方式，docToolchain 通过其用户的需求和想象力演进。

### 1.1 问题陈述

下图描述了 docToolchain 需要解决的问题：

![图 6.1：问题陈述机制](image/B15546_06_01.jpg)

*图 6.1：问题陈述机制*

当软件开发团队创建软件系统时，会使用构建系统创建编译后的构件，然后部署到运行时系统（参见 `Figure 6.1`）。

你可以将同样的方法应用于你的文档。这称为 `Docs-as-Code` 方法；也就是说，你像对待代码一样对待你的文档。为此，你需要多个辅助脚本来自动化构建文档的重复任务；为每个项目从头开始设置这些任务是浪费时间。

docToolchain——本文件中描述的系统——通过提供一套可以轻松安装的构建脚本来解决此问题。它们为你提供了一组任务来解决标准的 `Docs-as-Code` 需求。

功能需求主要分为两类：
*   从专有文件格式导出内容以在 AsciiDoc 中使用
*   生成或转换不同的输出格式

这些随后被转化为架构需求：

`RQ1`: 构建系统集成

系统应易于集成到标准软件构建中，以便像对待代码一样对待文档。

`RQ2`: 易于修改和扩展

docToolchain 应易于修改和扩展。

文档工具链有很多不同的功能需求——每个用户都应能够根据自己的需求进行调整。

`RQ3`: 企业就绪

项目应在不同的企业环境中运行。它必须与不同的版本控制系统一起运行，并且无需直接访问互联网（例如，仅通过代理进行依赖解析）。

文档不仅对开源项目至关重要，对企业项目也至关重要。

`RQ4`: 操作系统无关

工具链应在 Windows、Linux 和 macOS 上运行。

这使开发人员能够在自己选择的 operating system 上使用 docToolchain。

`RQ5`: Maven 和 Gradle

工具链应可用于两个主要的构建系统，Maven 和 Gradle。

这确保大多数基于 JVM 的项目可以使用它。

#### 注

`RQ5` 目前已被放弃（仅实现了 Gradle）——请参阅 DD3：对 Maven 和 Gradle 的支持。通过混合构建的变通方法可用。

`RQ6`: 克隆、构建、测试并运行

项目应设置成在你克隆仓库后即可运行。

这是为了避免使用它或更改项目代码所需的特殊设置。

### 1.2 质量目标

主要的质量目标如下：
*   `QS1`: 保密性：此质量目标不是 docToolchain 特有的，但在安全方面非常重要。
由于系统不仅针对开源环境，还针对企业环境，我们需要确保在用户不知情的情况下不会将任何数据发送到第三方系统。例如，有许多基于服务器的文本分析工具和库会将待分析的文本发送到第三方服务器。像这样的工具和库不得使用。
*   `QS2`: 易用性：docToolchain 项目仍是一个年轻的开源项目，因此需要一个稳定且不断增长的用户群。实现这一目标最简单的方法是使 docToolchain 极易使用。
*   `QS3`: 可维护性：docToolchain 背后没有大公司——只有在业余时间推动其发展的用户。因此，开发人员应在 1 到 3 小时的时限内完成大部分使 docToolchain 发展的更改。

### 1.3 利益相关者

`OSS Contributors`: `OSS Contributors` ([https://github.com/docToolchain/docToolchain/graphs/contributors](https://github.com/docToolchain/docToolchain/graphs/contributors)) 需要了解架构，以便遵循决策并在正确的方向上发展系统。

贡献者大多是系统的用户，他们需要将 docToolchain 任务适配到自己的需求或需要额外的任务。

*   `General docToolchain users`: 这些主要是具有开发背景的软件架构师和技术作家。很可能 docToolchain 用户也会使用 `arc42` 并且熟悉它。
当前重点是那些在企业环境中工作的软件架构师用户，他们使用 JVM、Git 和 Gradle。
这些用户需要在文档中包含架构图，并在受限环境中工作（例如使用代理、在本地 Maven 仓库中、互联网访问受限等）。
他们还需要能够为不同的利益相关者创建不同的输出格式。
*   `Users not working with the JVM`: docToolchain 也引起了那些不主要在 JVM 上工作的用户的注意。这些用户需要像使用任何其他命令行工具一样使用 docToolchain。
*   `Readers`: 文档的主要目的是被需要特定信息的人阅读。

## VI.2 架构约束

以下是各种架构约束：
*   `C1`: 在任何地方运行：docToolchain 必须在用户开发所需的现代操作系统上运行，例如 Windows、Linux 和 macOS。
*   `C2`: 为 JVM 构建：docToolchain 为 JVM 构建。虽然提供的一些功能可能也与其他环境一起工作，但不能保证所有任务都如此。
*   `C3`: 企业就绪：docToolchain 必须在受限的企业环境中运行。它必须使用代理、最小依赖、受限的互联网访问等运行。
这也列为需求 `RQ3`。
*   `C4`: 无头：docToolchain 必须无头运行；也就是说，无需显示即可在构建服务器上运行。必须避免用户交互和 Windows 功能。

## VI.3 系统范围和上下文

本章描述了 docToolchain 设计的各种上下文，包括其功能范围。

### 业务上下文

下图显示了 docToolchain 运行的业务上下文：

