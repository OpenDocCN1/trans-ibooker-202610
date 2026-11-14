
# 从变量名中学习

## 阅读示例代码片段

本章将包含具体练习，帮助你深入理解示例代码片段，但当然，你可能希望在继续阅读前先对代码有个大致印象。一个很好的方法是现在使用上一章的练习，在我们深入领域之前，更多地了解这段代码的结构。

虽然使用 Google 很容易弄清更多关于代码领域的信息，但我建议你此时不要这样做。

## 背景

我们将要学习的示例代码来自 `Monaco`，即 `Visual Studio Code` 中的编辑器。

如果你想在浏览器或 IDE 中查看代码，可以在这里找到：

`https://github.com/hoathaivan/nextjs-monaco-ls/blob/d31e9482265eddb70459e0a253b85d7c94bcce80/public/monaco-editor/esm/vs/editor/common/model/bracketPairsTextModelPart/colorizedBracketPairsDecorationProvider.js`

## 从变量名收集领域见解

在上一章中，你折叠了所有代码，以便只关注代码的结构元素。在理解代码领域的第一个技巧中，你将做相反的事情：你只需列出所有领域元素，而忽略代码的其余部分，包括结构组件。虽然我们可以在 IDE 中阅读代码，但它们最终主要是作为代码*编写*工具而创建的，因此通常不包含仅在代码阅读时有用的功能，例如创建一个列表，列出文件或项目中使用的所有不同标识符名称的选项。因此，我们将需要付出一些人工努力。

### 练习 3.1 列出代码库中的所有名称

对于这个练习，只需列出代码库中的所有名称。这里的名称是指程序员（而非语言设计者）选择的所有事物，包括变量、参数、类和函数/方法。记录每个变量的类型，以便于后续处理。

只关注名称，不要花费太多时间，我们编译这个列表是为了在本章后面进一步使用它。

| Name | Type |
| --- | --- |
| `ColorizedBracketPairsDecorationProvider` | `Class` |
| `bracketsInRange` | `local` |
| `getInlineClassName()` | `Method` |

## 示例代码片段

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
