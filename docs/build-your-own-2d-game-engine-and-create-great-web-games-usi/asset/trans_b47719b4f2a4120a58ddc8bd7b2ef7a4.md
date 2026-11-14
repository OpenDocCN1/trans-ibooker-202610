
随着第 [6](#334805_2_En_6_Chapter.xhtml) 章技术的加入，你现在拥有了一组关键量级的行为，它们可以组合创造出真正有趣的游戏机制，涵盖从动作游戏到益智游戏的全谱系。当然，游戏机制行为只是游戏设计九大要素之一，通常单靠它们不足以创造出神奇的游戏体验：背景设定、视觉风格、元游戏元素等都有重要贡献。好消息是，创造难忘的游戏体验不必像你通常认为的那样复杂精细，基于第 [1](#334805_2_En_1_Chapter.xhtml) 至 [6](#334805_2_En_6_Chapter.xhtml) 章涵盖的行为和技术的相对基础组合，依然能制作出伟大的游戏。最耀眼的游戏并不总是最复杂的，而是往往那些设计九大要素的每个方面都经过深思熟虑、和谐共处的游戏。如果你对游戏设计的各个方面给予适当的关注和专注，无论你是独自开发还是身处大型团队，你都已踏上制作佳作的正轨。

© The Author(s), under exclusive license to APress Media, LLC, part of Springer Nature 2022 K. Sung et al. *Build Your Own 2D Game Engine and Create Great Web Games* [https://doi.org/10.1007/978-1-4842-7377-7_7](https://doi.org/10.1007/978-1-4842-7377-7_7) Begin Abstract

# 7. 操控摄像机

Kelvin Sung^([1](#334805_2_En_7_Chapter.xhtml#Aff5)), Jebediah Pavleas^([2](#334805_2_En_7_Chapter.xhtml#Aff6)), Matthew Munson^([3](#334805_2_En_7_Chapter.xhtml#Aff7)) and Jason Pace^([4](#334805_2_En_7_Chapter.xhtml#Aff8)) (1) Bothell, WA, USA (2) Kenmore, WA, USA (3) Lake Forest Park, WA, USA (4) Portland, OR, USA

End Abstract

完成本章后，你将能够：

*   实现操控 `Camera`（摄像机）时常用的操作
*   在旧值和新值之间插值以创建平滑过渡
*   理解某些运动或行为如何用简单的数学公式描述
*   构建具有多摄像机视图的游戏
*   将鼠标点击的像素位置转换为 `World Coordinate (WC)`（世界坐标）位置
*   在具有多摄像机的游戏环境中编程处理鼠标输入

## 简介

你的游戏引擎现在能够表示和绘制对象。有了上一章介绍的基本抽象机制，引擎还能支持这些对象的交互和行为。本章将关注点重新聚焦于控制和交互 `Camera` 对象，该对象抽象并促进了游戏对象在画布上的呈现。这样，你的游戏引擎将能够控制和操控具有良好结构行为的视觉吸引力游戏对象的呈现。

图 [7-1](#334805_2_En_7_Chapter.xhtml#Fig1) 简要回顾了第 [3](#334805_2_En_3_Chapter.xhtml) 章介绍的 `Camera` 对象抽象。`Camera` 对象允许游戏程序员定义要显示在 HTML 画布 `viewport`（视口）中的游戏世界的 `World Coordinate (WC)`（世界坐标）窗口。`WC window` 由 `WC center` 和 `W_wc × H_wc` 的尺寸定义边界。`viewport` 是 HTML 画布上的一个矩形区域，其左下角位于 (`V_x`, `V_y`)，尺寸为 `W_v × H_v`。`Camera` 对象的 `setViewAndCameraMatrix()` 函数封装了细节，使得位于 `WC window bounds`（世界坐标窗口边界）内的所有游戏对象的绘制能够显示在相应的 `viewport` 中。 ![Figure 7-1 ¶ Review of WC parameters that define a Camera object](images/334805_2_En_7_Chapter/334805_2_En_7_Fig1_HTML.png)

Figure 7-1 ¶ Review of WC parameters that define a Camera object

> **Note**
>
> 在本书中，`WC window` 或 `WC bounds` 均用于指代 `WC window bounds`。

`Camera` 对象抽象让游戏程序员能够忽略 `WC bounds` 和 HTML 画布的细节，专注于设计有趣且娱乐性的游戏体验。在游戏关卡中使用 `Camera` 对象编程，应反映现实世界中使用实体摄像机的方式。例如，你可能想平移摄像机向观众展示环境，可能想将摄像机固定在女演员身上与观众分享她的旅程，或者你可能想扮演导演，指示场景中的演员保持在摄像机的视觉范围内。这些例子的显著特征，如平移或跟随角色视角，是高层功能规格。注意，在现实世界中，你不需要指定坐标位置或窗口边界。

本章介绍一些最常见的摄像机操控操作，包括限制、平移和缩放。将推导以插值形式呈现的解决方案，以缓解摄像机操控导致的恼人或困惑的突兀过渡。你还将学习在同一游戏关卡中支持多摄像机视图以及处理鼠标输入。

## 摄像机操控

在 2D 世界中，你可能希望将对象的移动限制在 `Camera` 的边界内，或者平移 `Camera`，又或者向特定区域缩放 `Camera` 或拉远 `Camera`。这些高层功能规格可以通过策略性地更改 `Camera` 对象的参数来实现：`WC center` 和 `WC window` 的 `W_wc × H_wc`。关键在于为游戏开发者创建方便的函数，以便在游戏上下文中操控这些值。例如，不用直接增加/减少 `WC window` 的宽度/高度，而是可以为程序员定义 `zoom functions`（缩放函数）。

### 摄像机操控项目

该项目演示了如何通过操作 `Camera` 对象的 `WC center`、宽度和高度来实现直观的摄像机操控操作。你可以在图 [7-2](#334805_2_En_7_Chapter.xhtml#Fig2) 中看到该项目运行的示例。该项目的源代码定义在 `chapter7/7.1.camera_manipulations` 文件夹中。 ![Figure 7-2 ¶ Running the Camera Manipulations project](images/334805_2_En_7_Chapter/334805_2_En_7_Fig2_HTML.jpg)

Figure 7-2 ¶ Running the Camera Manipulations project

项目控制如下：

*   `WASD keys`：移动 Dye 角色（`Hero object` 主角对象）。注意当 `Hero object` 尝试移动超出 `WC bounds` 的 90% 时，`Camera WC window` 会更新以跟随 `Hero object`。
*   `Arrow keys`：移动 `Portal object`（传送门对象）。注意 `Portal object` 无法移动超出 `WC bounds` 的 80%。
*   `L/R/P/H keys`：选择 `Left minion`（左侧小兵）、`Right minion`（右侧小兵）、`Portal object` 或 `Hero object` 作为聚焦对象；`L/R keys` 还会将 `Camera` 中心设置在 `Left minion` 或 `Right minion` 上。
*   `N/M keys`：向 `Camera` 中心缩放或拉远。
*   `J/K keys`：缩放或拉远，同时确保当前聚焦对象的相对位置保持不变。换句话说，随着 `Camera` 缩放，所有对象的位置都会改变，除了聚焦对象的位置。

项目目标如下：

*   体验一些常见的摄像机操控操作
*   理解从操控操作到必须修改的相应摄像机参数值的映射
*   实现摄像机操控操作

