# 序言

Angular 是一个强大的 Web 框架，可帮助团队构建健壮且可扩展的应用。Angular 不断发展，但其核心价值始终如一：构建一个开发者乐于使用的平台，让开发者能够构建用户乐于使用的应用，并打造一个人人都感到受欢迎的社区。

*Angular 项目* 是一本践行这些价值的书，它采用独特的方法，赋能新手和有经验的开发者使用 Angular 构建出色的应用。Aristeidis 采用项目驱动的方法，帮助学习者以引人入胜且易于理解的方式掌握现代 Angular 技术。

本指南将帮助学习者使用强大的 Angular CLI 构建他们的第一个应用，利用 Angular 强大的 PWA 集成，使用 Scully 构建高性能的静态应用，等等。对于希望充分发挥 Angular 体验价值的开发者来说，本书将带来显著的价值。

随着 Angular 继续为 Google 内部及整个 Web 带来非凡体验，本书将成为希望构建出色应用的开发者的绝佳学习资源。

*Mark Thompson*

*Google Angular 团队*

# 贡献者

## 关于作者

**Aristeidis Bampakos** 是一位经验丰富的前端 Web 开发者，也是 Angular 框架的 Google 开发者专家。从 2011 年起，他主要专注于使用 AngularJS 和 Angular 框架、TypeScript 以及 Angular Material 开发应用。他目前在 Plex-Earth 工作，专攻 Angular 开发，同时也在 Code.Hub 担任 Angular 高级技术讲师，向其他开发者和个人教授 Angular 框架。

> *我感谢所有支持和信任我的人，尤其是我的妻子 Ria 和我的好朋友 Jeffrey Bosch，感谢他们鼓励我，从未让我放弃。*

> *我还要感谢 Scully 团队的 Sander Elias 和 Ionic 团队的 Mike Hartington 在本书编写过程中给予的帮助。*

以下是他们的评价：

> *在我看来一切都很棒。干得漂亮！*

- Sander Elias

> *总体来说，非常不错。一切都解释得很清楚。干得好！*

- Mike Hartington

# 关于审校者

**Fabio Biondi** 是一位前端讲师、社区领袖，也是许多意大利 JavaScript 用户组的常客贡献者。

他是 Angular 的 Google 开发者专家和 Microsoft MVP，在多个 Web 技术领域拥有超过 15 年的企业级应用、交互体验、UIKit 和组件开发经验。

他是 Twitch 合作伙伴，还运营着一个 YouTube 频道，经常在上面发布技术内容。

## 前言

Angular 是一个流行的 JavaScript 框架，可以在广泛的平台上运行，包括 Web、桌面和移动端。它开箱即用地提供了丰富的功能和广泛的工具，深受开发者欢迎。这本更新后的第二版《Angular 项目实战》将教你如何使用 Angular 构建高效且优化的 Web 应用程序。

你将首先通过创建 10 个不同的真实 Web 应用程序来探索框架的核心特性。每个应用程序都将演示如何将 Angular 与不同的库和工具集成。随着学习的深入，你将学习如何实现 Angular Router、Scully、Electron、Angular 的 service worker、Nx 的 monorepo 工具、NgRx 等流行技术，同时构建问题跟踪系统、PWA 天气应用程序、移动照片地理标记应用程序、组件 UI 库以及其他许多令人兴奋的项目。在最后的章节中，你将掌握使用 schematics 自定义 Angular CLI 命令的方法。

读完本书后，你将具备根据自己或客户的需求，使用各种不同技术构建 Angular 应用程序的技能。

## 本书读者对象

如果你是一名具有 Angular 初级经验的开发者，并且希望熟练掌握应对 Angular 可能遇到的各种用例所需的基本工具，那么这本 Angular 开发书籍非常适合你。我们假设你具备 Web 应用程序开发的初级知识，并有使用 ES6 或 TypeScript 的基本经验。

## 本书涵盖内容

*第 1 章，在 Angular 中创建你的第一个 Web 应用程序，* 探讨了 Angular 框架的主要特性，并介绍了构成典型 Angular 应用程序的基本构建块。你将研究 Angular 生态系统中可用的不同工具和 IDE 扩展，以增强开发者的工作流程和体验。

*第 2 章，使用 Scully 和 Angular Router 构建 SPA 应用程序，* 介绍了 Angular 应用程序如何基于**单页应用程序（SPA）**架构，通常我们有多个页面由不同的 URL 或路由提供服务。另一方面，Jamstack 是一种新兴的热门技术，它允许你构建快速的静态网站，并直接从 CDN 提供服务。

在本章中，我们将使用 Angular Router 在 Angular 应用程序中实现路由功能。我们还将使用 Scully（Angular 最佳的静态站点生成器）来创建一个采用 Jamstack 架构的个人博客。

*第 3 章，使用响应式表单构建问题跟踪系统，* 我们将构建一个问题跟踪管理系统，并使用 Angular 响应式表单向系统添加新问题。我们将使用 VMware 的 Clarity Components 设计表单，并加入内置和自定义验证。我们还将响应表单中的值变化并相应地采取行动。

*第 4 章，使用 Angular Service Worker 构建 PWA 天气应用程序，* 介绍了 Web 应用程序的用户体验并非对所有用户都相同，尤其是在网络覆盖和连接性较差的地区。当我们构建 Web 应用程序时，应考虑各种网络类型。

在本章中，我们将创建一个使用 OpenWeather API 显示指定区域天气的应用程序。我们将学习如何将应用程序部署到 Firebase Hosting。我们还将探索使用 Angular service worker 的 PWA 技术，以在离线时提供无缝的用户体验。

*第 5 章，使用 Electron 构建桌面 WYSIWYG 编辑器，* 探讨了 Electron，这是一个跨平台的 JavaScript 框架，用于使用 Web 技术构建桌面应用程序。当与 Angular 结合时，它可以产生性能非常高的应用程序。

在本章中，我们将创建一个可以在桌面上运行的 WYSIWYG 编辑器。我们将构建一个 Angular 应用程序，并将其与 ngx-wig（一个流行的 WYSIWYG Angular 库）集成，然后使用 Electron 将其打包为桌面应用程序。数据通过 Node.js API 本地持久化在文件系统中。

*第 6 章，使用 Capacitor 和 3D 地图构建移动照片地理标记应用程序，* 介绍了 Capacitor，这是 Ionic 框架提供的一项服务，可以将任何 Web 应用程序（例如使用 Angular 创建的应用程序）转换为原生应用程序。其主要优势在于我们可以使用相同的代码库构建原生移动应用程序和 Web 应用程序。Cesium 是一个用于构建 3D 地图的流行 JavaScript 框架。

在本章中，我们将使用 Capacitor 为我们的照片构建一个地理标记移动应用程序。我们将使用各种 Ionic 插件在指定位置拍照并将其持久化到 Cloud Firestore。然后，我们将在 Cesium 3D 查看器中显示所有拍摄照片的列表。

*第 7 章，使用 Angular 为 GitHub 作品集构建 SSR 应用程序，* 深入探讨了**搜索引擎优化（SEO）**，这是当今任何网站的关键方面。谁不希望自己的网站在通过社交媒体分享时看起来很好？客户端 Web 应用程序的真正挑战是优化它，这可以通过在服务器上渲染内容来实现。

在本章中，我们将学习如何使用 GitHub API 创建 GitHub 作品集应用程序。然后，我们将在服务器上渲染它，并学习如何将状态传输到浏览器。我们还将了解如何动态设置页面标题和其他元数据，并使用 preboot 来管理页面转换。

*第 8 章，使用 Nx Monorepo 工具和 NgRx 构建企业门户，* 介绍了 monorepo 架构，这是一种在单个仓库中处理多个应用程序时流行的技术，为开发过程提供了速度和灵活性。

在本章中，我们将使用 Nx monorepo 开发工具创建两个门户：一个面向最终用户，他们可以选择一个**兴趣点（POI）**并在地图上访问它；另一个面向管理员，用于查看给定 POI 的访问统计。应用程序状态使用 NgRx 管理。

*第 9 章，使用 Angular CLI 和 Angular CDK 构建组件 UI 库，* 阐述了企业组织通常需要可在不同 Web 应用程序中使用的自定义 UI 库。Angular CDK 提供了广泛的功能，用于创建可访问且高性能的 UI 组件。

在本章中，我们将使用 Angular CDK 和 Bulma CSS 框架创建两个不同的组件。我们还将它们打包为一个 Angular 库，并学习如何将它们发布到 npm，以便在不同的应用程序中重用。我们还将研究如何将每个组件用作 Angular 元素。

*第 10 章，使用 Schematics 自定义 Angular CLI 命令，* 介绍了组织在创建组件或服务等 Angular 实体时通常会遵循不同的准则。Angular schematics 可以通过扩展 Angular CLI 命令并提供自定义自动化来帮助他们。

在本章中，我们将学习如何使用 Angular schematics API 构建我们自己的一组用于生成组件和服务的命令。我们将构建一个用于创建包含 Tailwind CSS 框架的 Angular 组件的 schematic。我们还将构建一个默认使用内置 HTTP 客户端的 Angular 服务。





## 充分利用本书

你需要在计算机上安装 Angular 12 的一个版本，最好是最新的次要版本。所有代码示例均在 Windows 上的 Angular 12.0.0 中测试过，但它们也应该适用于 Angular 12 的任何未来版本。

| 本书涵盖的软件/硬件 | 操作系统要求 |
|---|---|
| Angular 12 | Windows、macOS 或 Linux |
| Visual Studio Code | Windows、macOS 或 Linux |
| Scully | Windows、macOS 或 Linux |
| Clarity Design | Windows、macOS 或 Linux |
| Angular PWA | Windows、macOS 或 Linux |
| Electron | Windows、macOS 或 Linux |
| Ionic | Windows、macOS 或 Linux |
| CesiumJS | Windows、macOS 或 Linux |
| Firebase | Windows、macOS 或 Linux |
| GitHub API | Windows、macOS 或 Linux |
| Nx monorepo tools | Windows、macOS 或 Linux |
| NgRx | Windows、macOS 或 Linux |
| Angular Material | Windows、macOS 或 Linux（任意） |
| Angular CDK | Windows、macOS 或 Linux（任意） |
| Angular schematics | Windows、macOS 或 Linux（任意） |

**如果你使用的是本书的数字版本，我们建议你亲自输入代码，或者从书的 GitHub 仓库获取代码（下一节提供了链接）。这样做将帮助你避免与复制粘贴代码相关的任何潜在错误。**

## 下载示例代码文件

你可以从 GitHub 的 `https://github.com/PacktPublishing/Angular-Projects-Second-Edition` 下载本书的示例代码文件。如果代码有更新，它会在 GitHub 仓库中更新。

我们还有来自我们丰富的图书和视频目录中的其他代码包，可在 `https://github.com/PacktPublishing/` 获取。去看看吧！

## 下载彩色图片

我们还提供了一个 PDF 文件，其中包含本书中使用的截图和图表的彩色图片。你可以在此处下载：`https://static.packt-cdn.com/downloads/9781800205260_ColorImages.pdf`。

## 排版约定

本书通篇使用了许多文本约定。

文本中的代码：表示文中的代码字、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 句柄。以下是一个示例：“它将把 Clarity 库的所有必要 npm 包添加到 package.json 文件的 dependencies 部分。”

代码块的格式如下：

```html
<div class="main-container">
  <div class="content-container">
    <div class="content-area">
      <app-issue-list></app-issue-list>
    </div>
  </div>
</div>
```

当我们希望您注意代码块的特定部分时，相关行或项目会以粗体显示：

```json
"styles": [
  "node_modules/@clr/ui/clr-ui.min.css",
  "src/styles.css"
]
```

任何命令行输入或输出均按如下方式编写：

```bash
ng add @angular/material --theme=indigo-pink --typography=true --animations=true
```

**粗体：** 表示新术语、重要词语或你在屏幕上看到的文字。例如，菜单或对话框中的文字以**粗体**显示。以下是一个示例：“单击**添加新问题**按钮并输入新问题的详细信息。”

> **提示或重要说明**
> 像这样显示。

## 联系我们

我们始终欢迎读者的反馈。

**一般反馈：** 如果你对本书的任何方面有疑问，请发送电子邮件至 `customercare@packtpub.com`，并在邮件主题中提及书名。

**勘误：** 尽管我们已尽一切努力确保内容的准确性，但错误仍可能发生。如果你在本书中发现了错误，我们将不胜感激，请向我们报告。请访问 `www.packtpub.com/support/errata` 并填写表单。

**盗版：** 如果你在互联网上以任何形式发现我们作品的非法副本，我们将不胜感激，请向我们提供位置地址或网站名称。请通过 `copyright@packt.com` 联系我们，并附上材料的链接。

**如果你有兴趣成为作者：** 如果你有擅长的主题，并且有兴趣撰写或参与编写一本书，请访问 `authors.packtpub.com`。

## 分享您的想法

读完*Angular 项目（第二版）*后，我们很想听听您的想法！请点击此处直接进入本书的亚马逊评论页面并分享您的反馈。

您的评论对我们和技术社区都很重要，将帮助我们确保提供卓越质量的内容。
