
请记住，Web Components 是一组 Web 平台规范。因此，它们很可能长期被 Web 浏览器使用，并提供许多好处，包括以下内容：

- Web Components 可重用且可在框架之间工作。
- Web Components 可在所有主要 Web 浏览器中运行。
- Web Components 易于维护并为未来做好准备，主要因为它们基于 Web 平台规范。

## Web Components 生态系统的基本概念

在整本书中，我将使用一些与技术、方法或模式相关的术语，这些术语在我们的 Web 应用中使用 Web Components 时可以应用。如下所示。

## 设计系统

设计系统是可重用组件、指南和工具的目录或集合，使组织中的团队能够更高效地构建数字产品，并为所有产品应用一致的品牌形象。采用此方法的一些示例如下：

- *Google*：Material Design (https://material.io)
- *Adobe*：Spectrum (https://spectrum.adobe.com)
- *Ionic*：Ionic Framework (https://ionicframework.com/docs)

## 组件驱动开发

组件驱动开发意味着通过构建独立组件来设计软件应用程序。每个组件都有一个接口或 API 与系统的其余部分通信。使用此方法的一些优点是

- *更快开发*：将开发分离为组件允许你构建具有小范围和小目标的模块化部分。这意味着你可以更快地开发，并更快地为测试提供一个可供在其他系统中重用的部分。
- *更简便的维护*：当你必须添加或更新应用程序的功能时，你只需更新组件，而不必重构应用程序中更重要的部分。
- *可重用性*：模块化组件允许可重用功能，可以扩展以构建多个应用程序，从而消除反复重写的需要。
- *测试驱动开发 (TDD)*：实现单元测试以验证每个模块化组件的聚焦功能变得容易得多。
- *对系统的更好理解*：当系统由模块化组件组成时，更容易掌握、理解和操作。

## Web Components 的浏览器支持

在撰写本文时（2020 年初），Web Components 已被所有主要 Web 浏览器支持（见图 1-1）。

![](img/e276a43da8123ca0974f10d5c85948b9_15_0.png)

图 1-1. 支持 Web Components 主要规范的主要浏览器

## 入门

要开始使用 Web Components 构建应用，你必须了解并安装几种技术和工具。

## cmder（仅限 Windows）

cmder 是 Windows 的终端模拟器。默认情况下，Windows 操作系统自带一个终端（命令提示符），对开发并不实用。这就是为什么我们需要 cmder，它是一个模拟器，可让我们在终端中顺畅地运行命令。

- 要访问此模拟器，请前往 cmder.net 并下载最新版本。
- 将文件解压到你的 C:/ 位置。
- 进入 系统属性 ► 环境变量 并编辑 Path 变量，如图 1-2 所示。

![](img/e276a43da8123ca0974f10d5c85948b9_16_0.png)

图 1-2. 系统属性中的环境变量首选项

将 cmder 位置添加到 Path 变量中，如图 1-3 所示。

![](img/e276a43da8123ca0974f10d5c85948b9_17_0.png)

图 1-3. 系统属性中的 Path 变量首选项

从“选择命令提示符”运行 cmder，以测试环境变量（图 1-4）。

![](img/e276a43da8123ca0974f10d5c85948b9_17_1.png)

图 1-4. 从“选择命令提示符”运行 cmder

## Node.js

Node.js 是一个 JavaScript 运行时环境。大多数使用 JavaScript 的项目都使用 Node 来安装依赖项并创建脚本以自动化开发工作流。

你必须在你的机器上安装 Node。你可以从 https://nodejs.org/en/ 下载。

下载安装程序后，运行它并按照说明进行操作。

### 适用于 Mac

如果你使用的是 Mac，请按照图 1-5 所示的 Node.js 安装程序说明进行操作。

![](img/e276a43da8123ca0974f10d5c85948b9_18_0.png)

图 1-5. Node.js Mac 安装

然后打开你的终端并运行 $ node -v。如果一切正常，你将在终端中看到 Node 版本，如图 1-6 所示。

![](img/e276a43da8123ca0974f10d5c85948b9_19_0.png)

图 1-6. 终端中的 Node 版本

### 适用于 Windows

要为 Windows 安装 Node.js，请按照图 1-7 所示的安装程序说明进行操作。

![](img/e276a43da8123ca0974f10d5c85948b9_19_1.png)

图 1-7. Node.js Windows 安装

然后，完成后，打开 cmder 并运行 $ node -v。

## npm

安装 Node.js 时，你也会安装 npm。npm 是 Node.js 的包管理器，允许用户安装依赖项并在其 JavaScript 项目中运行小型脚本。

### 适用于 Mac 和 Windows

通过在终端中运行 $ npm -v 检查 npm 版本。如果一切正常，你将在终端中看到 npm 版本，如图 1-8 所示。

![](img/e276a43da8123ca0974f10d5c85948b9_20_0.png)

图 1-8. 终端中的 npm 版本

## Google Chrome

Chrome 是一款提供对 Web Components 优秀支持的 Web 浏览器，并包含 Chrome DevTools，这对开发者来说是一个方便的功能。你可以从 www.google.com/chrome/ 下载并安装 Chrome。

### 适用于 Mac 和 Windows

要为 Mac 和 Windows 安装 Chrome，运行安装程序并按照相关步骤操作。Chrome 安装成功后，打开它，你将看到欢迎屏幕（图 1-9）。

![](img/e276a43da8123ca0974f10d5c85948b9_21_0.png)

图 1-9. Google Chrome 安装

## Chrome DevTools

Chrome DevTools 是包含在 Google Chrome 浏览器中的一组 Web 开发者工具。此工具帮助你作为开发者诊断应用中的问题并使其更快。打开时，按 Command+Option+J（Mac）或 Control+Shift+J（Windows、Linux、Chrome OS），直接跳转到控制台面板（图 1-10）。

![](img/e276a43da8123ca0974f10d5c85948b9_22_0.png)

图 1-10. Google Chrome DevTools

## Lighthouse

Lighthouse 是一个用于改善网页质量的开源自动化工具。Lighthouse 可在 Chrome 的 DevTools 中找到。² 前往“审核”标签页访问它（图 1-11）。

²Google Developers，“Lighthouse”，https://developers.google.com/web/tools/lighthouse/，访问日期 2020 年 9 月 28 日。

![](img/e276a43da8123ca0974f10d5c85948b9_23_0.png)

图 1-11. Google Chrome DevTools 审核标签页

## Vue

本书中的一些示例将使用 Vue.js 框架。Vue.js 是一个简单且令人耳目一新的 JavaScript 框架。Vue 主要面向视图层，但你可以添加所需的内容，并利用其生态系统中的所有工具构建强大的渐进式 Web 应用。

在你的项目中使用 Vue 非常容易。你只需要在 index.html 中添加以下内容，如清单 1-1 所示。

### 清单 1-1. 从 CDN 开发版本添加 Vue

```
<!-- development version, includes helpful console warnings -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

或添加生产版本，如清单 1-2 所示。

### 清单 1-2. 从 CDN 生产版本添加 Vue

```
<!-- production version, optimized for size and speed -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

## Vue CLI

Vue CLI 是一个用于快速 Vue.js 开发的完整系统。借助此工具，我们在处理 Webpack、EsLint 和其他工具时可以避免一些额外工作，专注于在应用中构建业务逻辑。你必须通过在终端中运行以下命令在系统中安装它：$npm install -g @vue/cli。

如果一切正常，你将在终端中看到 Vue CLI 版本，如图 1-12 所示。

![](img/e276a43da8123ca0974f10d5c85948b9_24_0.png)

图 1-12. 终端中的 Vue CLI 版本

## Git

Git 是一个设计用于处理项目中不同更改的版本控制系统。我们将使用 Git 来操作我们的 Web 应用项目并处理各章中概述的后续步骤。你可以从 https://git-scm.com/downloads 下载并安装 Git。

### 适用于 Mac 和 Windows

要安装，运行安装程序并按照步骤操作。完成后，打开 cmd.exe/终端并运行 $ git --version。

如果一切正常，你将在终端中看到 Git 版本，如图 1-13 所示。
