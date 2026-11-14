

## 参考文献

*   Johnson, Riess, and Arnold. *Introduction to Linear Algebra*, 5th edition. Addison-Wesley, 2002.
*   Anton and Rorres. *Elementary Linear Algebra: Applications Version*, 11th edition. Wiley, 2013.

## 技术

以下列表提供了获取本书所用技术更多信息的链接：

*   **JavaScript**: [www.w3schools.com/js](http://www.w3schools.com/js)
*   **HTML5**: [www.w3schools.com/html/html5_intro.asp](http://www.w3schools.com/html/html5_intro.asp)
*   **WebGL**: [www.khronos.org/webgl](http://www.khronos.org/webgl)
*   **OpenGL**: [www.opengl.org](http://www.opengl.org)
*   **Visual Studio Code**: [https://code.visualstudio.com/](https://code.visualstudio.com/)
*   **Chrome**: [www.google.com/chrome](http://www.google.com/chrome)
*   **glMatrix**: [http://glmatrix.net](http://glmatrix.net)
*   **ESLint**: [www.eslint.org](http://www.eslint.org)

© The Author(s), under exclusive license to APress Media, LLC, part of Springer Nature 2022  
K. Sung et al. *Build Your Own 2D Game Engine and Create Great Web Games*  
[https://doi.org/10.1007/978-1-4842-7377-7_2](https://doi.org/10.1007/978-1-4842-7377-7_2)

# 2. 使用 HTML5 和 WebGL

**Kelvin Sung**<sup>(1)</sup>, **Jebediah Pavleas**<sup>(2)</sup>, **Matthew Munson**<sup>(3)</sup>, 和 **Jason Pace**<sup>(4)</sup>  
(1) Bothell, WA, USA  
(2) Kenmore, WA, USA  
(3) Lake Forest Park, WA, USA  
(4) Portland, OR, USA

## 摘要

完成本章后，你将能够：

*   为你的简单游戏引擎创建一个新的 JavaScript 源代码文件
*   使用 WebGL 绘制一个简单的纯色正方形
*   定义 JavaScript 模块和类来封装并实现核心游戏引擎功能
*   理解抽象的重要性以及组织源代码结构以支持复杂度增长的重要性

## 简介

绘图是所有视频游戏共有的最基本功能之一。游戏引擎应为其绘图系统提供灵活且对程序员友好的接口。这样，在构建游戏时，设计师和开发者可以专注于游戏本身的重要方面，如机制、逻辑和美术。

WebGL 是一种现代 JavaScript 图形应用程序编程接口 (API)，专为基于 Web 浏览器的应用程序设计，通过直接访问图形硬件提供高质量和高效率。基于这些原因，WebGL 作为支持游戏引擎绘图的基础非常出色，特别是对于设计为通过互联网进行游玩的视频游戏。

本章探讨使用 WebGL 绘图的基础知识，设计抽象以封装无关细节从而便于编程，并构建基础设施来组织复杂的源代码系统以支持未来的扩展。

> **注意**  
> 本书中你将开发的游戏引擎基于最新版本的 WebGL 规范：版本 2.0。为简洁起见，术语 WebGL 将用于指代此 API。

## 用于绘图的 Canvas

要绘图，你必须首先在网页中定义并划出一个专用区域。你可以通过使用 HTML `<canvas>` 元素来轻松实现这一点，该元素用于定义一个用于 WebGL 绘图的区域。`<canvas>` 元素是一个绘图容器，你可以使用 JavaScript 访问和操作它。

### HTML5 Canvas 项目

该项目演示如何在网页上创建和清除 `<canvas>` 元素。图 [2-1](#334805_2_En_2_Chapter.xhtml#Fig1) 显示了运行该项目的示例，该项目定义在 `chapter2/2.1.html5_canvas` 文件夹中。

![图 2-1 ¶ 运行 HTML5 Canvas 项目](images/334805_2_En_2_Chapter/334805_2_En_2_Fig1_HTML.jpg)

**图 2-1** ¶ 运行 HTML5 Canvas 项目

项目目标如下：

*   学习如何设置 HTML `<canvas>` 元素
*   学习如何从 HTML 文档检索 `<canvas>` 元素以供 JavaScript 使用
*   学习如何从检索到的 `<canvas>` 元素创建 WebGL 引用上下文，并通过 WebGL 上下文操作 canvas

### 创建和清除 HTML Canvas

在第一个项目中，你将创建一个空的 HTML5 canvas 并使用 WebGL 将 canvas 清除为特定颜色：

1.  **创建一个新项目**，方法是在你选择的目录中创建一个名为 `html5_canvas` 的新文件夹，并复制粘贴你在第一章前一个项目中创建的 `index.html` 文件。

> **注意**  
> 从这一点开始，当要求创建新项目时，你应该遵循前面描述的过程。即，创建一个以项目名称命名的新文件夹，并复制/粘贴前一个项目的文件。这样，你的新项目可以在保留原有功能的基础上进行扩展。

2.  **在编辑器中打开 `index.html` 文件**，方法是打开 `html5_canvas` 文件夹，如果需要则展开它，然后点击 `index.html` 文件，如图 [2-2](#334805_2_En_2_Chapter.xhtml#Fig2) 所示。

![图 2-2 ¶ 在项目中编辑 index.html 文件](images/334805_2_En_2_Chapter/334805_2_En_2_Fig2_HTML.jpg)

**图 2-2** ¶ 在项目中编辑 `index.html` 文件

3.  **创建用于绘图的 HTML canvas**，方法是在 `index.html` 文件的 `<body>` 元素中添加以下行：

```html
<canvas id="GLCanvas" width="640" height="480">
    Your browser does not support the HTML5 canvas.
</canvas>
```

该代码定义了一个名为 `GLCanvas` 的 `<canvas>` 元素，并指定了 `width` 和 `height` 属性。正如你稍后将体验到的，你将检索对 `GLCanvas` 的引用以绘制到该区域。元素内部的文本将在你的浏览器不支持使用 WebGL 绘图时显示。

> **注意**  
> `<body>` 和 `</body>` 标签之间的行被称为“在 body 元素内”。在本书其余部分，“在 *AnyTag* 元素内”将用于指代开始标签 (`<AnyTag>`) 和结束标签 (`</AnyTag>`) 之间的任何行。

4.  **创建一个用于包含 JavaScript 编程代码的 `<script>` 元素**，同样在 `<body>` 元素内：

```html
<script type="text/javascript">
    // JavaScript code goes here.
</script>
```

这完成了该项目的 HTML 部分。你现在将为示例的其余部分编写 JavaScript 代码。

5.  **在 JavaScript 代码中检索对 `GLCanvas` 的引用**，方法是在 `<script>` 元素内添加以下行：

```javascript
"use strict";
let canvas = document.getElementById("GLCanvas");
```

> **注意**  
> JavaScript 关键字 `let` 用于定义变量。  
> 第一行，`use strict`，是一条 JavaScript 指令，指示代码应在“严格模式”下执行，在该模式下使用未声明的变量会导致运行时错误。第二行创建一个名为 `canvas` 的新变量，并将该变量引用到 `GLCanvas` 绘图区域。

> **注意**  
> 所有局部变量名均以小写字母开头，如 `canvas`。

6.  **检索并绑定对 WebGL 上下文的引用到绘图区域**，方法是添加以下代码：

```javascript
let gl = canvas.getContext("webgl2") ||
         canvas.getContext("experimental-webgl2");
```

正如代码所示，检索到的 WebGL 版本 2 上下文引用存储在名为 `gl` 的局部变量中。通过此变量，你可以访问 WebGL 2.0 的所有功能。再次强调，在本书其余部分，术语 WebGL 将用于指代 WebGL 版本 2.0 API。

7.  **通过 WebGL 将 canvas 绘图区域清除为你喜欢的颜色**，方法是添加以下内容：

