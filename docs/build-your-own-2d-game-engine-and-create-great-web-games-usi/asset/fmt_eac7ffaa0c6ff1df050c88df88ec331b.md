
# 书籍概览

## 第 2–4 章：基础设施构建

**第 2 章** 建立初始基础设施，将源代码系统分离为包含以下内容的文件夹和文件：JavaScript 专用的核心引擎逻辑、WebGL2 GLSL 专用的着色器程序，以及 HTML5 专用的网页内容。这种组织方式允许在保持源代码系统局部化变更的同时，持续扩展引擎功能。例如，引入游戏对象行为增强时，只需修改 JavaScript 源代码文件。

**第 3 章** 构建绘图框架，封装并隐藏 WebGL2 绘图细节，使其与引擎其他部分隔离。该绘图框架支持游戏对象行为的开发，而无需分心于它们如何被绘制。

**第 4 章** 引入并集成核心游戏引擎功能组件，包括游戏循环、键盘输入、高效的资源与关卡加载，以及音频支持。

## 第 5–7 章：游戏引擎基本功能

**第 5 章** 专注于纹理映射的使用，包括精灵表、基于精灵表的动画，以及位图字体的绘制。

**第 6 章** 提出游戏对象及其行为的抽象，包含逐像素精确的碰撞检测。

**第 7 章** 详述摄像机的操作与交互，包括多摄像机编程与鼠标输入支持。

## 第 8–11 章：进阶功能提升

**第 8 章** 涵盖二维游戏场景中三维光照效果的模拟。

**第 9 章** 讨论基于物理的行为模拟。

**第 10 章** 展示适合建模爆炸效果的粒子系统基础。

**第 11 章** 探讨更高级的摄像机功能，包括基于平铺的无限滚动与视差效果。

## 第 12 章：完整游戏设计总结

第 12 章 通过引导你基于已开发的游戏引擎设计一个完整游戏，总结全书内容。

# 代码示例

本书每章都包含示例，供你交互式地实验与学习新知识。你可以通过点击 [www.apress.com/9781484273760](http://www.apress.com/9781484273760) 页面上的 **Download Source Code** 按钮，获取所有项目的源代码及关联资源（图片、音频片段或字体）。你将看到按章节编号组织的文件夹结构。每个文件夹内包含对应本书各节的 Visual Studio Code (VS Code) 项目子文件夹。

# 致谢

本书项目直接源于作者在 **Game-Themed CS1/2: Empowering the Faculty** 项目中构建游戏的学习经验，该项目由 **Transforming Undergraduate Education in Science, Technology, Engineering, and Mathematics (TUES)** 计划、**National Science Foundation (NSF)** 资助（奖项编号 DUE-1140410）。我们要感谢 NSF 项目官员 Suzanne Westbrook 对我们项目的信任，以及 Jane Prey、Valerie Bar 和 Paul Tymann 的鼓励。

第二版的出版得益于众多学生和合作者的推动。特别感谢华盛顿大学博塞尔分校 **CSS452: Game Engine Development** 课程（参见 [https://myuwbclasses.github.io/CSS452/](https://myuwbclasses.github.io/CSS452/)）的学生们，他们提出了最严厉、要求最高，却又最具支持性的反馈。通过众多游戏与 API 扩展项目（参见 [https://html5gameenginegroup.github.io/GTCS-Engine-Student-Projects/](https://html5gameenginegroup.github.io/GTCS-Engine-Student-Projects/)），我们明确认识到必须更新 JavaScript 与 WebGL (Web Graphics Library) 版本、底层同步机制，最重要的是物理引擎的覆盖范围。

Fernando Arnez（首版合著者）教会了我们 JavaScript。Yaniv Schwartz 指引我们了解 JavaScript async/await 与 Promise。与 Huaming Chen 和 Michael Tanaya 的讨论与合作，直接促成了游戏引擎物理章节的完成。Akilas Mebrahtom 和 Donald Hawkins 构建了第 9 章末尾的额外示例，演示了常见物理材质的潜在预设。音量控制由 Kyla NeSmith 首先研究并集成。Nicholas Carpenetti 和 Kyla NeSmith 为初始游戏引擎开发了用户界面 API，遗憾的是未收录于此版。这些及无数其他反馈共同提升了本书内容的质量与改进。

贯穿全书示例项目的主角 **Dye** 及众多视听素材，基于教学对象与面向对象层级概念而设计的 **Dye Hard** 游戏。Dye Hard 原始开发团队成员包括：Matthew Kipps、Rodelle Ladia、Chuan Wang、Brian Hecox、Charles Chiou、John Louie、Emmett Scout、Daniel Ly、Elliott White、Christina Jugovic、Rachel Harris、Nathan Evers、Kasey Quevedo、Kaylin Norman-Slack、David Madden、Kyle Kraus、Suzi Zuber、Aina Braxton、Kelvin Sung、Jason Pace、Rob Nash。Kyle Kraus 创作了第 4 章 **Audio Support** 项目中使用的背景音乐，原为教授循环而设计的 **Linx** 游戏所作。第 12 章游戏的背景音频由 David Madden 作曲、Aina Braxton 编排。感谢 Clover Wai 提供的图表与插图。

我们还要感谢 Spandana Chatterjee 对我们理念的信任、耐心，以及持续高效有力的支持。衷心感谢 Mark Powers 的勤奋与闪电般的邮件回复。Mark 应该学会并考虑偶尔睡一会儿觉。Nirmal Selvaraj 统筹一切，确保项目有序推进。

最后，感谢 Yusuf Pisan 对技术审阅给出的深刻、高效、且极快的反馈。

本著作中的所有观点、发现、结论与建议仅代表作者个人，不一定反映赞助方的观点。

# 作者简介

Kelvin Sung
