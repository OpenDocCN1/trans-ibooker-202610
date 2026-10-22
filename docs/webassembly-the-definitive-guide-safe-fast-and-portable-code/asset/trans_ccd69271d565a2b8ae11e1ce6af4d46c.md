

# O'REILLY®

# WebAssembly

# 权威指南

安全、快速且可移植的代码

![](img/16b7990eb0dbf589db177bdbd2f4a189_0_0.png)

# 早期发布
RAW & UNEDITED

Brian Sletten

# WebAssembly: The Definitive Guide

安全、快速且可移植的代码

通过 Early Release 电子书，您可以在这些书籍的官方发布之前就获得书籍的最早形式——作者在写作时的原始未编辑内容，以便您可以在这些技术正式发布之前就利用它们。

# Brian Sletten

# WebAssembly: The Definitive Guide

Brian Sletten 著

Copyright © 2022 Bosatsu Consulting, Inc.. All rights reserved.

Printed in the United States of America.

由 O’Reilly Media, Inc. 出版，地址：1005 Gravenstein Highway North, Sebastopol, CA 95472。

O’Reilly 的书籍可用于教育、商业或销售推广用途。大多数标题的在线版也可用（http://oreilly.com）。如需更多信息，请联系我们的企业/机构销售部门：800-998-9938 或 corporate@oreilly.com。

收购编辑：Suzanne McQuade

开发编辑：Angela Rufino

制作编辑：Kate Galloway

校对：Piper Editorial Consulting, LLC

室内设计：David Futato

封面设计：Karen Montgomery

插图：Kate Dullea

December 2021: First Edition

# 早期发布的修订历史

- 2020-11-05: First Release
- 2020-11-16: Second Release
- 2021-02-04: Third Release
- 2021-03-10: Fourth Release
- 2021-03-23: Fifth Release
- 2021-05-04: Sixth Release
- 2021-10-08: Seventh Release

有关发布详情，请参阅 http://oreilly.com/catalog/errata.csp?isbn=9781492089841。

O’Reilly 标志是 O’Reilly Media, Inc. 的注册商标。
*WebAssembly: The Definitive Guide*、封面图像和相关商品外观均为 O’Reilly Media, Inc. 的商标。

本作品中表达的观点是作者的观点，不代表出版商的观点。虽然出版商和作者已尽力确保本作品中包含的信息和说明准确无误，但出版商和作者不承担因使用或依赖本作品而产生的任何错误或遗漏的责任，包括但不限于因使用或依赖本作品而产生的损害。本作品中包含的信息和说明的使用风险自负。如果本作品中包含或描述的任何代码示例或其他技术受开源许可或他人知识产权的约束，则您有责任确保您对该技术的任何使用均符合此类许可和/或权利。

978-1-492-08977-3

## 致谢

本书诞生于特权之山，在许多人没有在家办公的奢侈的时期。因此，本书献给在黑暗时期保持灯火通明的前线和 essential 工人。

## 前言

### 致早期发布读者的说明

通过 Early Release 电子书，您可以在这些书籍的官方发布之前就获得书籍的最早形式——作者在写作时的原始未编辑内容，以便您可以在这些技术正式发布之前就利用它们。

如果您对如何改进本书的内容和/或示例有任何意见，或者如果您注意到本章中缺少材料，请通过 wasmbook@bosatsu.net 与作者联系。

我相信 WebAssembly 是一种正在崛起的技术，有潜力以某种形式彻底改变整个软件开发行业。我并不认为 WebAssembly 之所以具有变革性是因为我正在撰写一本关于该主题的书籍。我之所以撰写这本书，是因为我相信它将具有变革性。

 presumably 您也对该技术感兴趣。问题是，我认为我对您作为读者的了解比许多作者都要少。如果这是一本关于特定编程语言或特定主题的书籍，读者会有自我选择性，我就可以继续进行。但 WebAssembly 的主题比大多数人意识到的要大得多，我正试图用这本书描绘一幅非常大的图景。迄今为止出版的大多数其他书籍都专注于其单一方面，我可以理解为什么。

你们中的一些人可能认为它是一种杀死 JavaScript 的技术。它不是。你们中的一些人可能认为它关乎将应用程序带到浏览器。它确实是这样，但它也是如此之多。它在服务器端、视频游戏世界中、作为插件机制、在支持无服务器函数和边缘计算方面、在嵌入式系统、区块链以及我们将一起研究的许多其他主题中都很有用。这是据我所知首次尝试对该主题进行如此全面的尝试，我认为这很重要，也是时候讲述这个更完整的版本了。

在本书出版之前，我与谈论该项目的人大多得到了积极的支持和兴奋。 我得到的有限的反对意见之一是关于标题。一些人认为对于如此新的技术来说，“权威指南”这个称号为时过早。那是一个公平的立场，但由于我试图描述一个极其庞大和包罗万象的技术环境，我认为这是合理的。我希望到本书结束时你会同意。

我唯一的要求是您保持开放的心态并稍有耐心。WebAssembly 涉及大量语言、运行时和运行环境。除了教您有关低级细节的知识外，我们还将研究与该领域的主流编程语言的集成以及几种不同的用例。我已尽量不对您的背景做出太多假设，因此我在文本中大量添加了脚注形式的进一步探索和发现的面包屑。如果您是一位更高级的开发人员，只寻求有关 WebAssembly 的详细信息，请随意忽略这些内容，不要介意。我预计会有相当广泛的受众至少会浏览这本书，我也希望他们感到受欢迎。

如果您在开发方面属于初级水平，这将是一本具有挑战性的书。但我已尽力让您至少了解发生了什么。把各种链接和参考资料当作通往更复杂的开发现实的个人指南。不要感到不知所措，只需一次处理一个问题，按您感兴趣的顺序或有意义的顺序进行即可。进入这个行业没有唯一的方式，无论您如何进入都是合法的。

归根结底，WebAssembly 将允许我们基本上选择我们的编程语言，并在几乎任何计算表面区域安全地运行它们。我们之前被承诺过这一点，但我认为这次更有可能实现。感谢您给我机会解释原因。

## 本书使用的约定

本书中使用了以下排版约定：

*Italic*

表示新术语、URL、电子邮件地址、文件名和文件扩展名。

*Constant width*

用于程序列表，以及在段落中引用程序元素，如变量或函数名、数据库、数据类型、环境变量、语句和关键字。

*Constant width bold*

显示用户应逐字键入的命令或其他文本。

*Constant width italic*

显示应由用户提供的值或由上下文确定的值替换的文本。

TIP
此元素表示提示或建议。

NOTE
此元素表示一般说明。

WARNING
此元素表示警告或注意。

## 使用代码示例

补充材料（代码示例、练习等）可在 https://github.com/bsletten/wasm_tdg 下载。

如果您对代码示例有技术问题或使用问题，请发送电子邮件至 bookquestions@oreilly.com。

本书旨在帮助您完成工作。一般来说，如果本书提供了示例代码，您可以在程序和文档中使用它。 除非您复制代码的很大一部分，否则您无需联系我们征求许可。例如，使用本书中几个代码片段编写程序不需要许可。出售或分发 O’Reilly 书籍中的示例需要许可。引用本书并引用示例代码来回答问题不需要许可。将大量示例代码纳入您产品的文档中需要许可。

我们感谢但通常不需要署名。署名通常包括标题、作者、出版商和 ISBN。例如：“WebAssembly The Definitive Guide by Brian Sletten(O’Reilly). Copyright 2022 Bosatsu Consulting, Inc., 978-1492089841.”

如果您认为您对代码示例的使用超出了合理使用范围或上述许可范围，请随时联系我们 permissions@oreilly.com。

## O’Reilly Online Learning

> NOTE
> 40 多年来，O’Reilly Media 一直为公司提供技术和商业培训、知识和见解，帮助公司取得成功。

