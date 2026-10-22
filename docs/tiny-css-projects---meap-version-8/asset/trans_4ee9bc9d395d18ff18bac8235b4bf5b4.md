

# 可访问性与文本对齐

警告：网络内容无障碍指南（WCAG）包含三个相互递进的符合性等级 A、AA 和 AAA。A 是限制最少的，逐步提升到最严格的 AAA。通常网站会以 AA 等级为目标，但如果我们需要符合 AAA，则值得一提的是，文本两端对齐违反了无障碍指南 1.4.5，而该条款是 AAA 的要求⁴。

要对齐我们的文本，我们将使用 `text-align` 属性，该属性可以取 left、right、center 或 justify 的值。我们将 `text-align: justify;` 添加到之前创建的段落规则中。

我们的段落规则现在有 2 个属性，text-align 和 line-height，这可以处理段落的样式。列表 7.3 显示了完成的段落规则。

## 列表 7.3 完成的段落规则

```
p {
  line-height: 1.5;
  text-align: justify;
}
```

段落本身已处理完毕，如图 7.3 所示，我们现在可以专注于第一段的第一字母来创建首字下沉

![](img/b56145aa9b751926a7aedbd7172bc818_194_0.png)

图 7.3 样式化的段落

## 7.1.3 首字母

我们不需要向 HTML 添加任何标签来选择第一段的首字母，我们可以使用伪类 `:first-of-type` (https://developer.mozilla.org/en-US/docs/Web/CSS/:first-of-type) 来选择第一段，然后使用伪元素 `::first-letter` (https://developer.mozilla.org/en-US/docs/Web/CSS/::first-letter) 来获取该字母，在本例中为 B，两者都可以串联。在代码中，这被翻译为 `p:first-of-type::first-letter {}`。

## 伪类 vs 伪元素

伪类添加到选择器中以定位特定状态，而伪元素允许我们选择元素的一部分。

选中字母后，我们现在可以开始对其进行样式设置，使其看起来像首字下沉。为了使其从其余文本中脱颖而出，我们将选择一种更华丽的字形。在本例中，我们将使用 Passions Conflict (https://fonts.google.com/specimen/Passions+Conflict) 如图 7.4 所示，我们将从 Google Fonts 导入该字体。

![](img/b56145aa9b751926a7aedbd7172bc818_195_0.png)

图 7.4 Passions Conflict 字形

由于这种字体的字母大写特别华丽，因此非常适合用于我们的首字下沉。我们稍后在本章中也会使用它来设置本章开头引用的样式。像这样的漂亮字体是装饰页面的绝佳方式，但仅适用于非常短的内容。手写体和装饰字体可能很难阅读，因此不适合用于大量文本。然而，对于首字下沉、大标题或简短引用，它们可以成为区分元素与内容其余部分并为页面增添个性的好方法。

这种特定字体中的字形比 Times New Roman（我们用于其余内容的字体）的字形小得多，而我们正在创建的首字下沉按定义比其余文本更大，因此我们必须调整其大小。我们还将调整字母的行高，使其与文本很好地配合。最后我们将使其向左浮动，以便文本环绕字母，达到我们期望的效果。

float 属性根据传递给它的值将元素放置在其容器的右侧或左侧。周围的内联元素（我们的文本）将使用剩余空间环绕浮动元素。float 属性可以取以下三个属性之一：

- Left
- Right
- None（元素未浮动）

由于我们的文本是英文，从左到右流动，我们希望字母 B 保持在左侧。因此，我们将第一段的首字母（B）向左浮动，通过在规则中添加 `float: left;`。列表 7.4 显示了我们创建的用于设置首字下沉样式的完整 CSS 规则以及 Passions Conflict 字体的导入。图 7.5 显示了生成的输出。

## 列表 7.4 首字下沉

```
@import url(
  'https://fonts.googleapis.com/css2?family=Passions+Conflict&display=swap'
);
p:first-of-type::first-letter {
  font-size: 6em;
  float: left;
  line-height: .5;
  font-family: 'Passions Conflict', cursive;
}
```

A Passions Conflict 字体的导入
B 样式化我们第一段开头字母 B 的规则

![](img/b56145aa9b751926a7aedbd7172bc818_196_0.png)

图 7.5 首字下沉

我们使用 em 和无单位的行高，这样如果我们 ever 更改段落的字体大小，首字下沉也会相应缩放，因为 1 em 的值是基于父元素的 font-size 设置的，在本例中是我们的段落标签。

为了重新定位我们的 B 使其与文本很好地配合，我们编辑了字母的行高。另一种我们可以使用的方法是将 B 的位置设置为 relative，然后使用 top、bottom、left 和 right 来相对于其余文本更改其位置。

首字下沉创建后，我们现在将注意力转向本章开头的引用。

## 7.2 样式化引用

页面顶部的引用目前相当单调，而且在文本中有些迷失。为了使其脱颖而出，我们将使用与首字下沉相同的字体。由于前面提到的大小和行高差异，我们将对其进行调整，使段落和引用看起来大小和间距均匀。列表 7.6 显示了我们将添加以完成此任务的 CSS，而列表 7.6 显示了输出。

```
Listing 7.5 Blockquote
```css
blockquote {
  font-family: 'Passions Conflict', cursive;
  font-size: 2em;
  line-height: 1;
}
```
```

![](img/b56145aa9b751926a7aedbd7172bc818_197_0.png)

图 7.6 引用块

我们再次使用相对单位，这样如果其余内容的字体大小发生变化，引用也会随之变化。你可能已经注意到，我们使用了行高 1，尽管我们上面说过为了最佳可读性，行高应为 1.5 到 2。我们在这里破例，因为观察输出，我们发现默认情况下该字体本身已有非常大的行高，我们不需要增加大小。在处理草书或装饰字体时，这种情况偶尔会发生。

文本处理完毕后，我们现在可以专注于图像。

## 7.3 使文本围绕指南针弯曲

要使文本环绕我们的指南针图像，我们需要做的第一件事是将指南针向右浮动。我们的指南针是 png，由于是图像，因此本质上是矩形的，因此文本环绕图像遵循矩形路径。图 7.7 显示了浮动的指南针。图像上已应用边框以显示其边界框。

![](img/b56145aa9b751926a7aedbd7172bc818_198_0.png)

Buck lived at a big house in the sun-kissed Santa Clara Valley. Judge Miller's place, it was called. It stood back from the road, half hidden among the trees, through which glimpses could be caught of the wide cool veranda that ran around its four sides. The house was approached by gravelled driveways which wound about through wide-spreading lawns and under the interlacing boughs of tall poplars. At the rear things were on even a more spacious scale

图 7.7 方形指南针

## 7.3.1 Shape-Outside: Circle

为了使文本跟随指南针的曲线，我们需要为文本环绕添加一个曲线。

我们将使用的属性是 shape-outside (https://developer.mozilla.org/en-US/docs/Web/CSS/shape-outside)。shape-outside 允许我们定义一个形状，相邻文本将围绕该形状流动。该形状也不必是矩形。它可以是

- Circle 或椭圆
- Polygon
- 源自图像 - 使用图像的 alpha 通道（透明度）来确定形状应为
- Path - 在规范中，但在本书撰写时尚未在任何浏览器上实现 (https://developer.mozilla.org/en-US/docs/Web/CSS/basic-shape#path())
- Box model 值

由于我们有一个非常圆形的图形，我们要实现的目标形状是一个圆。这给了我们几个选项：

