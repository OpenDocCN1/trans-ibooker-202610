## 第 2 章 深入理解代码的业务领域

上一章在不深入代码内容的情况下考察了代码中的关系。本章将反其道而行之，在不考察连接方式的情况下审视代码内容。我们将专门关注代码业务领域内的术语和概念，换句话说，就是代码所要解决的现实问题。

当你阅读陌生信息（如陌生代码）时，你的大脑会同时进行许多不同的活动。你试图利用工作记忆去*理解*结构，以及所有部分如何相互关联。在上一章中，我们学习了在考察关系时支持工作记忆的技巧。

但你也会在*回忆*已存储在长期记忆中的信息。并行做这些事非常消耗精力，会增加理解代码的难度。因此，在进行调试或审查等更复杂的活动之前，专注于陌生代码片段中的领域概念，并确认没有困惑，这一点很重要。

本章并非对所有代码阅读目的都有用。如果你正在处理一个领域对你非常熟悉的代码库，例如因为你在某个领域（如保险或养老金）工作了很长时间，领域概念不会让你的大脑不堪重负。但如果你刚开始接触新客户的代码库，或者试图了解一个使用新术语的开源项目，从代码中考察和学习领域概念将极大地减轻后续与代码打交道的难度。

因此，本章仅考察代码中存在的业务领域概念。此处的领域是指代码的业务领域或问题。编程领域将在第 4 章介绍。考察领域概念很有用，因为如果你错过了对代码中词汇确切含义的透彻理解，就很难整体理解代码。这与你的长期记忆有关，正如第 2 章所讨论的：如果你缺乏一些先验理解，记忆和分析代码将比必要时困难得多。

变量、方法或函数名可以成为代码领域方面的重要信息来源，因为它们通常包含领域中使用的词汇。在《*企业应用架构模式*》一书中，Martin Fowler 将领域模型描述为包含两个组成部分：行为和数据。^(1) 变量中出现的词汇通常指向数据或行为：动词通常指代行为，而名词指代对象。因此，从某种意义上说，在本章中你将关注对象及其功能，但你将从名称而非代码结构中收集信息。

本章的练习和技巧因此仅关注代码的领域方面，帮助你回答诸如以下问题：该代码库中的核心领域概念是什么？它们如何相互关联？

读完本章后，你将掌握 X 种技巧，可用于更深入地理解代码库的领域：

*   列出所有变量名
*   识别变量名的不同部分
*   查看命名模式

### 贯穿示例

正如上一章一样，本章我们将使用一个示例代码片段。即使它看起来完全令人不知所措，也别担心！我们将逐步讲解。

#### 示例代码：Monaco Editor 中的括号着色



### 从变量名中学习

#### 阅读示例代码片段

本章将包含具体练习，帮助你深入理解示例代码片段，但当然，你可能希望在继续阅读前先对代码有个大致印象。一个很好的方法是现在使用上一章的练习，在我们深入领域之前，更多地了解这段代码的结构。

虽然使用 Google 很容易弄清更多关于代码领域的信息，但我建议你此时不要这样做。

#### 背景

我们将要学习的示例代码来自 `Monaco`，即 `Visual Studio Code` 中的编辑器。

如果你想在浏览器或 IDE 中查看代码，可以在这里找到：

`https://github.com/hoathaivan/nextjs-monaco-ls/blob/d31e9482265eddb70459e0a253b85d7c94bcce80/public/monaco-editor/esm/vs/editor/common/model/bracketPairsTextModelPart/colorizedBracketPairsDecorationProvider.js`

#### 从变量名收集领域见解

在上一章中，你折叠了所有代码，以便只关注代码的结构元素。在理解代码领域的第一个技巧中，你将做相反的事情：你只需列出所有领域元素，而忽略代码的其余部分，包括结构组件。虽然我们可以在 IDE 中阅读代码，但它们最终主要是作为代码*编写*工具而创建的，因此通常不包含仅在代码阅读时有用的功能，例如创建一个列表，列出文件或项目中使用的所有不同标识符名称的选项。因此，我们将需要付出一些人工努力。

##### 练习 3.1 列出代码库中的所有名称

对于这个练习，只需列出代码库中的所有名称。这里的名称是指程序员（而非语言设计者）选择的所有事物，包括变量、参数、类和函数/方法。记录每个变量的类型，以便于后续处理。

只关注名称，不要花费太多时间，我们编译这个列表是为了在本章后面进一步使用它。

| Name | Type |
| --- | --- |
| `ColorizedBracketPairsDecorationProvider` | `Class` |
| `bracketsInRange` | `local` |
| `getInlineClassName()` | `Method` |

#### 示例代码片段

```typescript
/*---------------------------------------------------------------------------------------------
*  Copyright (c) Microsoft Corporation. All rights reserved.
*  Licensed under the MIT License. See License.txt in the project root for license information.
*--------------------------------------------------------------------------------------------*/
import { Emitter } from '../../../../base/common/event.js';
import { Disposable } from '../../../../base/common/lifecycle.js';
import { Range } from '../../core/range.js';
import { editorBracketHighlightingForeground1, editorBracketHighlightingForeground2, editorBracketHighlightingForeground3, editorBracketHighlightingForeground4, editorBracketHighlightingForeground5, editorBracketHighlightingForeground6, editorBracketHighlightingUnexpectedBracketForeground } from '../../core/editorColorRegistry.js';
import { registerThemingParticipant } from '../../../../platform/theme/common/themeService.js';

export class ColorizedBracketPairsDecorationProvider extends Disposable {
   constructor(textModel) {
       super();
       this.textModel = textModel;
       this.colorProvider = new ColorProvider();
       this.onDidChangeEmitter = new Emitter();
       this.onDidChange = this.onDidChangeEmitter.event;
       this.colorizationOptions = textModel.getOptions().bracketPairColorizationOptions;
       this._register(textModel.bracketPairs.onDidChange(e => {
           this.onDidChangeEmitter.fire();
       }));
   }
   //#region TextModel events
   handleDidChangeOptions(e) {
       this.colorizationOptions = this.textModel.getOptions().bracketPairColorizationOptions;
   }
   //#endregion
   getDecorationsInRange(range, ownerId, filterOutValidation) {
       if (ownerId === undefined) {
           return [];
       }
       if (!this.colorizationOptions.enabled) {
           return [];
       }
       const result = new Array();
       const bracketsInRange = this.textModel.bracketPairs.getBracketsInRange(range);
       for (const bracket of bracketsInRange) {
           result.push({
               id: `bracket${bracket.range.toString()}-${bracket.nestingLevel}`,
               options: {
                   description: 'BracketPairColorization',
                   inlineClassName: this.colorProvider.getInlineClassName(bracket, this.colorizationOptions.independentColorPoolPerBracketType),
               },
               ownerId: 0,
               range: bracket.range,
           });
       }
       return result;
   }
   getAllDecorations(ownerId, filterOutValidation) {
       if (ownerId === undefined) {
           return [];
       }
       if (!this.colorizationOptions.enabled) {
           return [];
       }
       return this.getDecorationsInRange(new Range(1, 1, this.textModel.getLineCount(), 1), ownerId, filterOutValidation);
   }
}

class ColorProvider {
   constructor() {
       this.unexpectedClosingBracketClassName = 'unexpected-closing-bracket';
   }
   getInlineClassName(bracket, independentColorPoolPerBracketType) {
       if (bracket.isInvalid) {
           return this.unexpectedClosingBracketClassName;
       }
       return this.getInlineClassNameOfLevel(independentColorPoolPerBracketType ? bracket.nestingLevelOfEqualBracketType : bracket.nestingLevel);
   }
   getInlineClassNameOfLevel(level) {
       // To support a dynamic amount of colors up to 6 colors,
       // we use a number that is a lcm of all numbers from 1 to 6.
       return `bracket-highlighting-${level % 30}`;
   }
}

registerThemingParticipant((theme, collector) => {
   const colors = [
       editorBracketHighlightingForeground1,
       editorBracketHighlightingForeground2,
       editorBracketHighlightingForeground3,
       editorBracketHighlightingForeground4,
       editorBracketHighlightingForeground5,
       editorBracketHighlightingForeground6
   ];
   const colorProvider = new ColorProvider();
   collector.addRule(`.monaco-editor .${colorProvider.unexpectedClosingBracketClassName} { color: ${theme.getColor(editorBracketHighlightingUnexpectedBracketForeground)}; }`);
   const colorValues = colors
       .map(c => theme.getColor(c))
       .filter((c) => !!c)
       .filter(c => !c.isTransparent());
   for (let level = 0; level < 30; level++) {
       const color = colorValues[level % colorValues.length];
       collector.addRule(`.monaco-editor .${colorProvider.getInlineClassNameOfLevel(level)} { color: ${color}; }`);
   }
});
```



一旦你列出了所有变量名，就可以开始更深入地探索领域。正如我们在练习 3.1 中所见，变量名通常由许多不同的部分组成，例如 `independentColorPoolPerBracketType`。

变量名通常包含单独的部分，如 `quantifiers`（量词）、`programming concepts`（编程概念）和 `domain concepts`（领域概念）。

### 量词

`Quantifiers`（量词）是指代数量的名称部分，如 `count`、`number_of`、`maximum` 或 `filtered`。有时这些量词还包含被量化的对象，如 `maximum_refund_per_month`。

### 编程概念

`Programming concepts`（编程概念）也会出现在名称中。例如，在 `m_object` 这样的名称中，`object` 可以指代语言中的特定概念。名称中指代概念的其他部分有 `list`、`hash`、`table`、`string` 或 `number`。在本章中，我们暂不关注概念；那是第 4 章的主题。

### 领域概念

`Domain concepts`（领域概念）指向领域，如 `customer`、`import`、`parser`、`code` 或 `language`。变量名的某些部分指向概念，通常以名词形式出现，如 `customer` 或 `code`，但它们也可以以动词形式表示 `domain behavior`（领域行为），如 `import` 或 `parse`。

通过拆解名称，你可以初步了解代码库中涉及哪些概念和行为。

### 自动提取领域概念

除了手动方法，你还可以使用工具来提取领域概念。虽然文本分析工具通常用于文本，但它们也可以应用于代码。例如：<https://pypi.org/project/wordcloud/>

一旦你收集了示例代码片段（或你正在调查的自己的文件或模块）中的所有名称，我们的下一步是确定代码中的哪些概念属于领域概念。因为如果我们想更好地理解领域，首先需要知道领域中有哪些概念。我们将从将变量名拆分为不同的单词开始，然后确定名称中的哪些单词是领域概念，这并不像你想象的那么简单。

首先，有时甚至很难找到这些部分。例如，考虑名称 `getInlineClassName()`。我们是在获取 `Inline Class`（内联类）的名称？还是在获取 `Class` 的 `Inline Name`（内联名称）？换句话说，`Inline` 是 `Class` 的属性还是 `Name` 的属性？你可能需要进一步阅读代码才能弄清楚！

而且，即使你能找到所有部分，什么是领域概念也不总是清晰的。例如，名称 `ColorizedBracketPairsDecorationProvider` 可以拆分为五个单词：`Colorized`、`Bracket`、`Pairs`、`Decoration` 和 `Provider`。

然而，如果不进一步了解这段代码的功能，就很难分辨什么是编程概念，什么是领域概念：`Decoration` 和 `Provider` 可能指代编程概念，`Pairs` 可能是一种数据类型，甚至 `Brackets` 也是代码中出现的东西。

因此，我们可能需要收集更多关于代码功能的知识，以便为练习 3.2 做出一些判断。看看你能理解多少，不要过度思考。如果你不确定，可以随意将概念放入 `Unclear`（不确定）列。

#### 练习 3.2：区分名称中的不同部分

从练习 3.1 的变量列表中，提取变量名中的单个单词，并将它们分为量词、编程概念和领域概念。如果你不确定，就把概念放在 `Unclear` 列中。

| 名称 | 量词 | 编程 | 领域 | 不确定 |
| --- | --- | --- | --- | --- |
| `ColorizedBracketPairsDecorationProvider` | | | `Provider` | `Colorized`, `Bracket Pair`, `Decoration` |
| | | | | |
| | | | | |
| | | | | |
| | | | | |

如果你能接触到领域专家（如果你在阅读开源项目的代码，这并不总是可行），这是一个讨论概念列表的好时机。值得提出的好问题包括：

- 领域概念列表中是否有可能不明显的同义词？
- 该列表是否遗漏了任何核心领域概念或行为？

### 确定领域概念之间的关系

一旦你对核心领域概念有了初步的了解，我们将探讨这些概念如何组合在一起，形成一个更连贯的图景，展示领域包含的内容。我们将使用第 2 章的术语，深入探讨迄今为止在练习 3.1 中收集的领域概念的类型。

在第 2 章中，我们区分了对象和函数，这种区分在领域维度中同样适用。在领域中，我们也有对象，如 `Decorations`，它们可以用在函数中，如 `getDecorationsInRange()`。在审视领域时，还有一个区分很有用：对象与对象属性之间的区别。像 `Bracket Pairs` 这样的对象可以拥有像 `Colorized` 这样的属性，因为在这个领域中，一对括号可以获得特定的颜色。所以 `Colorized` 本身不是一个领域概念，而是一个与 `Bracket Pairs` 一起使用的修饰符。

我们在练习 3.1 中收集的类型也有助于确定领域概念的组件类型。函数往往包含更多通用词汇，尤其是动词，如 `execute`、`read`、`load` 或 `create`。然而，函数名也经常揭示特定领域的行为，如 `buying`、`selling`、`placing an order` 或 `favoriting an item`。函数名不仅限于行为。函数名也可以揭示领域概念；例如，函数名 `getDecorationsInRange()` 告诉我们存在一个 `range`（范围），装饰物落在这个范围内。

#### 将概念映射到组件

将练习 3.1 中的所有名称及其类型，以及练习 3.2 中名称中的所有概念列在灰色列中。这些内容填入表格的前 3 个灰色列，并非新信息。现在填写最后一列，将概念分为 `Object`（对象）或 `Function`（函数）组件，或 `Attributes`（属性）。

##### 练习 3.3：概念与组件

| 名称 | 类型 (3.1) | 概念 (3.2) | 组件 |
| --- | --- | --- | --- |
| `ColorizedBracketPairsDecorationProvider` | `Class` | `Bracket Pair` | `Object` |
| | | `Colorized` | `Attribute (of Bracket Pair)` |
| | | `Decoration` | `Attribute (of Bracket Pair)` |
| | | | |
| | | | |
| | | | |

#### 寻找概念之间的关系

虽然概念的扁平列表有助于深入了解代码库中的核心概念，但可视化可以是一种更丰富的知识存储方式，这也更符合大脑处理信息的方式。大脑存储信息更像是网络，而不是扁平列表或层级结构，因此使这些联系显性化有助于获得更好的洞察并更好地记忆信息。

##### 练习 3.4：概念关系与映射

将概念列表与行为列表结合起来。哪些属于一组？

| 概念 → 操作 ↓ | `program` | `tree` | | |
| --- | --- | --- | --- | --- |
| `check if valid` | `x` | | | |
| `process` | | `x` | | |
| | | | | |
| | | | | |
| | | | | |

### 扩展知识

你现在已经查看了变量和函数或方法中出现的名称，并从名称中收集了两个领域类别：概念和行为。在总结你的发现之前，让我们分别关注这两个类别。阅读的下一步旨在尝试尽可能精确地定义概念和行为，基于你对代码和领域的先验知识，以及如果可能的话，基于文档和他人的知识。

#### 收集关于概念的更多知识



让我们首先关注代码中存在的概念。从概念列表开始，判断你是否能用自己的话解释领域概念的含义。

如果不能，阅读更多代码可能不是最好的下一步。相反，你可能想通过询问队友或其他贡献者，或阅读注释和文档，从其他来源收集更多关于这些概念的信息。

尤其有价值的是，还要调查你是否怀疑代码中使用了同义词。在编写这个练习时，我意识到在 Hedy 代码库中，我们有时说 `parse tree`，但在其他地方也使用 `tree`。虽然这对代码团队来说可能非常清楚，但使用同义词会让新手感到困惑。甚至使用同义词还会对经验丰富的开发人员产生负面影响，例如妨碍可搜索性，因为查找 "parse tree" 无法找到代码中 "tree" 的位置。

### 读后

#### 练习 3.5：描述概念

现在是重新审视你在练习 3.2 中从名称中收集到的概念的好时机。你是否足够了解该领域，以描述每个概念的含义？如果不，请在含义列中描述这一点，以便稍后在收集到更多信息时回到此列表。

| Concept | Meaning |
| --- | --- |
| `Bracket` | 一个括号，如果是右括号可能会出乎意料 |
| `Bracket Pair` | 两个括号是属于一起的代码，在编辑器中应获得相同的颜色 |
| … |  |
|  |  |
|  |  |

#### 丰富领域理解

一旦你调查了代码并努力从代码本身理解领域概念，用两种方式丰富你的知识将非常有价值。

首先，你可以将注意力转向代码库之外。根据你的上下文，这可能是深入文档以获取关于你在开源项目中发现的概念的更多信息，或者是与公司内部经验丰富的同事进行一次会议。在这两种情况下，你都是在寻求加深对刚刚了解的概念的理解。一个很好的练习是复查练习 3.2 和 3.4 的发现。你能验证练习 3.6a 中的含义和同义词是否正确，以及练习 3.6b 中的数据类型是否正确吗？

其次，在利用代码库外部的信息扩展了所有领域概念的知识后，加深理解的第二种方法是重读代码。现在是放弃对代码中名称的狭隘关注、整体重读代码的大好时机。因为你的知识现在扩展了，你一定会注意到代码中之前未曾注意到的新事物。

#### 扩展文档

关于领域概念和行为的一些信息可以作为添加到 readme 文件中的优秀文档。特别是，你已经探索了代码中许多重要概念，这些概念可能尚未存在于任何文档文件中。在某种意义上，你可能已经开始记录项目的 `Ubiquitous Language`（通用语言）。

##### 通用语言

Eric Evans 在其《领域驱动设计》一书中创造了 `Ubiquitous Language`（通用语言）一词。Evans 用通用语言指代开发人员和软件系统用户共享的语言。这种语言通常只有一部分自然产生，部分需要刻意设计。Evans 指出，`Ubiquitous Language` 应基于软件中使用的领域模型，因为自然语言概念往往不够精确。

你迄今收集的信息可以转化为文档。你在练习 3.6a 和 3.6b 中创建的概念列表可以作为领域概念列表的起点。从那里，你可以扩展，例如通过：

*   确保变量或函数名中的所有缩写或首字母缩略词要么被完整单词替代，要么如果人们倾向于保留它们，则进行记录
*   描述那些让你感觉相似但实际不同的领域对象。如果它们不完全相同但有看起来相似的原因，例如相似的名称或相似的行为，其他人也可能会感到困惑。记录这些概念的细微差别非常有用
*   与用户讨论这些列表。他们能识别并理解所有概念吗？缺少什么概念？它们是否可能缺失或以其他方式表示？
*   如果你在一个实践 DDD 并使用 `Ubiquitous Language` 的环境中工作，你可以检查你的概念列表是否与现有的 `Ubiquitous Language` 文档相似。

此外，你可能会发现一些名称不能准确描述代码功能，例如 `analyze()`、`convert()` 或 `process()`。在阅读代码时，你可能通过检查函数的调用跟踪、函数中的变量名或注释，对这些词有了更深入的理解。你或许可以致力于完善文档，特别是这些函数的 `docstrings`（文档字符串），以帮助未来的读者。

#### 讨论代码质量

在从领域角度审视代码时，你可能会发现代码中存在代码异味的部分，具体来说是语言学代码异味，即与对象、函数、方法或变量命名相关的代码异味。你的领域发现可以作为与团队讨论命名质量的绝佳切入点。

**相似对象**

你的领域分析可能指向看起来非常相似的对象。除了记录差异（如上所述），你可能还想深入探究代码如此的原因，例如通过讨论：

*   这些领域概念真的是同义词还是不同的？如果不同，有何不同？
*   相似性是否有历史原因？有时引入新产品或流程时，会带来自己的对象，因为作者可能不知道代码中哪些地方适合它。

**语言学代码异味**

第 2 章讨论了两种不同的代码异味：Fowler 描述的结构性异味，以及 Arnaoudova 定义的语言学代码异味。既然本章关注领域概念，按异味评估名称质量是合理的。你可以关注特定类型的语言学异味，Arnaoudova 在其著作中将其区分为函数/方法三种和标识符三种。

*   做得比说得多的方法
*   说得比做得多的方法
*   做与说相反的方法
*   名称暗示包含内容多于实体实际包含内容的标识符
*   名称暗示包含内容少于实体实际包含内容的标识符
*   名称暗示内容与实体实际包含内容相反的标识符



识别这些代码异味最简单的方法是从练习 3.1 和 3.3 制作的表格入手，对比两个可以了解类型的来源。首先，从名称中收集的类型（如 `find_id`，表明返回一个 id，可能是字符串或整数）与函数定义中定义的名称类型（如 `int find_id()`）进行对比。当然，在静态语言与动态语言中这会有很大不同。练习 3.6 可以帮助你对比这两个来源。

##### 练习 3.6 检查标识符和函数/方法的类型

在该练习中，检查你在练习 3.1 和 3.3 中创建的代码库中的标识符列表和函数/方法列表。对于每个标识符和函数/方法，根据名称识别预期类型，并通过阅读代码或从其他来源收集信息确定实际类型。如果你完成了练习 2.3（描述方法间的依赖关系），该表格也可能有助于你理解方法的角色和输出值。

目前，尝试关注代码的输出值，例如查看 `return` 关键字的位置或最后几行代码。在这个代码阅读阶段，你特别关注命名维度，而非深度阅读，以避免工作记忆过载。

| 标识符名称 | 基于名称预期的类型 | 从代码阅读收集的类型 |
| --- | --- | --- |
| `keywords_requiring_indentation` |  |  |
| `imported_product` |  |  |
| `…` |  |  |
| 函数或方法名称 |  |  |
| `process_input_string` |  |  |
| `check_program_size_is_valid` |  |  |
| `…` |  |  |

### 自动检查语言异味

你是否好奇代码库是否存在语言反模式？代码库是用 Java 编写的吗？如果是，你可以使用 Arnaoudova 基于研究创建的语言反模式检测器（LAPD）自动评估异味。LAPD 作为 Eclipse Checkstyle 插件的扩展可用。^(2)

#### 命名模式

命名模式是变量名中元素通常组合的模式（详见后文突出框），是检查代码质量的一个重要维度。在代码库中使用许多不同的命名模式会使代码不必要地难以阅读。如果你想检查代码库中的命名模式，你已经在练习 3.2 中完成了部分工作，当时你列出了名称及其单词/领域部分。现在你可以在下一个练习中扩展此表以检测不同的模式。

##### 练习 3.7 检查命名模式

检查代码库中标识符的名称，确定元素出现的顺序，如示例所示。这里我区分动词、名词、形容词和量词，但你可能想添加自己的类别，如首字母缩略词（EBIT）或过滤器（monthly, per customer）。

你能找到多少种不同的单词顺序？讨论出现的模式也很有趣。这些顺序或单词在局部或全局是否相似，即一个方法是否大量使用一种模式，而另一个方法使用不同的模式？

| 变量名 | 元素 | 顺序 |
| --- | --- | --- |
| `keywords_requiring_indentation` | `keyword, requiring, indentation` | `noun, verb, noun` |
| `imported_product` | `imported, product` | `adjective, noun` |
| `minimum_distance` | `minimum, distance` | `quantifier, noun` |
|  |  |  |
|  |  |  |
|  |  |  |

##### 注记

命名模式^(3)是变量名中元素通常组合的模式。例如，当需要为某人每月可获得的最大福利命名时，你可以选择 `max_benefit`、`max_benefit_per_month` 或 `max_benefit_num`。这些名称包含相同的领域概念，但表达方式不同。Feitelson 对 47 名开发者进行了实验，结果显示两名开发者选择相同名称的概率很低：研究中的中位数概率仅为 7%！多样化的变量名会给工作记忆带来更大压力，因为你每次都要在不同位置搜索相关部分。

#### 重构

如果你想利用本章的发现改进代码库，可以考虑以下重构，部分基于第 3.4.2 节的质量问题。

##### 发音相似的对象

在某些情况下，发音相似的对象可能需要代码变更。以下是几个问题：

*   如果它们真正不同，能否重命名一个以最小化重叠和困惑？
*   如果它们相同，能否合并为一个对象，共享一些相似性？

##### 模糊的名称

你可能会发现一些名称不能准确描述代码功能，如 `analyze`、`convert`。对于像 `process()` 这样模糊的名称，什么更精确的名称更好？

##### 异味名称

如果你在练习 3.9 中发现名称与实现不完全匹配，可能需要重命名方法或标识符使其更精确。你也可能想与更熟悉代码库的人讨论其实现，以确定名称为何不匹配实现。在某些情况下，修改实现比修改名称更好。例如，当你有一个名为 `return name_and_id` 的方法但只返回 id，然后在多处用它获取名称时，最好实际返回名称和 id，并重构代码以集中获取名称的逻辑。

### 总结

代码可以在不同维度阅读：结构、领域、概念、上下文和协作。本章探讨了领域维度，研究了变量、函数和对象的命名方式、从名称可以学到什么，以及如何利用名称理解和提高代码质量。下一章将探讨编程概念。

^(1) https://martinfowler.com/books/eaa.html

^(2) http://www.veneraarnaoudova.ca/linguistic-anti-pattern-detector-lapd/

^(3) Dror G. Fietelson et al., [“How Developers Choose Names”](https://www.cs.huji.ac.il/w~feit/papers/Names22TSE.pdf)
