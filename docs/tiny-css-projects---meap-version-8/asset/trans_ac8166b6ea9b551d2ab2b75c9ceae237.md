

## A Parent Item (.list)
## B Children of .list
## C Grand child of .list
## D Great grandchildren of .list

我们将只为列表项（子项）设置绯红色样式，而不影响嵌套的列表项（曾孙项）。因此浏览器会找到 `list` 类，并仅针对直接位于 `list` 类下的子列表项（`li`）进行定位，并将 `color` 改为 `crimson`。我们将使用以下 CSS：`.list > li { color: crimson; }`

正如我们在图 1.17 中看到的，列表项现在是绯红色的，而嵌套的列表项仍然是黑色的。这是使用子组合器的好处之一。它允许我们仅设置子元素的样式，而不设置孙元素或层次结构中更深层的元素。

![](img/b56145aa9b751926a7aedbd7172bc818_30_0.png)

图 1.17 子组合器应用于列表项

*那么我们可以反过来选择子元素的父元素，对吗？* 简短的答案是否定的。目前在 CSS 中没有办法选择父项，因此以下示例将不起作用：`article < p { color: blue; }`。

## 3. ADJACENT SIBLING COMBINATOR (+)

当我们需要为与另一个元素处于同一级别的元素设置样式时（就像你的兄弟姐妹在家族树中与你处于同一级别一样），我们可以使用相邻兄弟组合器。

如果我们想定位另一个元素之后的元素，可以使用相邻兄弟选择器。

在此示例中（列表 1.7），浏览器将找到 `header` 标签的任何用法，然后定位紧跟在 `header` 标签之后（或相邻）的第一个段落（`p`），并将 `font-size` 改为 `1.5rem`，`font-weight` 改为 `bold`。

```
Listing 1.8 Adjacent sibling combinator

header + p {
  font-size: 1.25rem;
  font-weight: bold;
}
```

图 1.18 显示了该代码应用于我们的文章。

# Title of our article (heading 1)

Posted on May 16 by Lisa.

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque tincidunt dapibus eleifend. Nam eu urna ipsum. Etiam consequat ac dolor et dapibus. Duis eros arcu, interdum eu volutpat ac, lacinia a tortor. Vivamus justo tortor, porttitor in arcu nec, pretium viverra ipsum. Nam sit amet nibh magna. Sed ut imperdiet orci, id finibus justo. Maecenas magna mauris, tempor nec tempor id, aliquam et nibh. Nunc elementum ut purus id eleifend. Phasellus pulvinar dui orci, sed eleifend magna ullamcorper sit amet. Proin iaculis lacus congue aliquam sodales.

- 1. List item 1
  - Nested item 1
  - Nested item 2
- 2. List item 2
- 3. List item 3
- 4. List item 4

![](img/b56145aa9b751926a7aedbd7172bc818_31_0.png)

Curabitur id augue nulla. Aliquam purus urna, aliquam eu ornare id, maximus et tellus. Aliquam eleifend sem vitae una blandit, non bibendum tellus dignissim. Aliquam imperdiet imperdiet sapien sit amet consectetur. Nam convallis turpis felis, sed vulputate lacus eleifend a. Mauris pharetra imperdiet lacinia. Sed sit amet feugiat lectus, in consectetur magna. Vestibulum accumsan porta enim ut ultricies. Vestibulum vitae massa quis massa dignissim imperdiet.

> Nunc eleifend nulla lobortis porta rhoncus. Vivamus feugiat, sem vitae feugiat aliquam, orci nulla venenatis libero, vitae rhoncus nibh neque ac velit.

Etiam tempor vulputate varius. Duis at metus ut eros ultrices facilisis. Donec ut est finibus, egestas nisl eu, placerat neque. Pellentesque cursus, turpis nec sollicitudin sodales, nisi

图 1.18 相邻兄弟组合器应用于 header 之后立即出现的段落

如果我们想让第一个元素与其他元素不同以使其脱颖而出，这可能会很有用。我们在看报纸时可能会看到这种情况。第一段可能会被设计得比其余部分更突出，以引起我们的注意。另一个用例是表单中的错误处理。相邻兄弟组合允许我们在表单控件中无效值之后立即向用户显示错误消息。

值得注意的是，与子组合器类似，我们无法找到前一个兄弟元素。`header + p { ... }` 找不到 header 之前的段落将不起作用。

## 4. GENERAL SIBLING COMBINATOR (~)

通用兄弟组合器比之前的方法更开放，因为它允许我们定位选择器所定位元素之后的所有兄弟元素。

在我们的示例中，我们将设置 `header` 元素之后出现的所有图像的样式。我们会注意到我们有三张占位符图像。第一张较小，可能是一个徽标或作者照片，位于 `header` 之上。我们不想为其设置样式。其他两张图像位于文章的更下方。我们希望在它们周围添加边框，以保持与文章其余部分一致的颜色主题。

我们的规则如下所示：`header ~ img { border: 4px solid crimson; }`。浏览器将找到 `header` 元素并定位该元素之后的所有兄弟图像（`img`）

element 并添加一个边框，其厚度为 4px，线条为实线（与点线、虚线或双线相对），颜色为绯红色。
我们可以在图 1.19 中看到该代码应用于我们的文章。

![](img/b56145aa9b751926a7aedbd7172bc818_32_0.png)

图 1.19 通用兄弟组合器定位 header 的兄弟图像

## 1.6.3 Pseudo-class and pseudo-element selectors

### PSEUDO-CLASS

伪类添加到选择器中以定位元素的特定状态。它们对于用户会交互的元素尤其有用，例如链接、按钮和表单字段。伪类使用单个冒号 (:) 后跟元素的状态。在我们的文章中有几个链接。我们没有以任何方式设置它们的样式，因此它们的样式来自用户代理样式表。对于大多数浏览器，它们有下划线，并且颜色会根据链接是否已被访问过（网址是否在浏览器的历史记录中找到）而有所不同。

对于链接，有几个状态需要考虑。最常见的是：

- link: 包含 href 属性且网址未在用户浏览器历史记录中找到的锚标签
- visited: 包含 href 且网址在用户浏览器历史记录中找到的锚标签
- hover: 用户将光标悬停在链接上，但尚未按下
- `active`: 用户正在点击链接并按住
- `focus`: 聚焦元素是默认情况下会接收键盘事件的元素。当用户点击元素时，该元素会自动获得焦点（除非有 JavaScript 更改此行为）。使用键盘在表单字段、链接和按钮之间导航也会更改聚焦的元素。
- `focus-within`: 应用于父元素。如果父元素的子元素获得焦点，则会应用 focus-within 样式。
- `focus-visible`: 仅在通过键盘导航获得焦点或用户通过键盘与元素交互时应用。

还有 `:target` 伪类，我们将在本书后面在讨论 `text-fragments` 时探索它。

在我们当前的文章项目中，我们将首先创建 `a:link` 规则，将包含 href 元素且尚未访问的锚标签的颜色更改为浅蓝色，使用其十六进制颜色代码 (#1D70B8)。

`:visited` 状态应与 `:link` 状态不同，因为它应向用户表明他们之前已访问过该页面。经常我们会看到网站在两者之间没有区别，然而区分两者可以提供更好的体验。

在我们的示例中，我们将 `:visited` 状态更改为紫色，使用十六进制代码值 #4C2C92。

我们将处理 `:hover` 状态。悬停状态不适用于移动用户，因为没有办法识别用户悬停在链接上。在我们的文章中，我们将 `:hover` 状态的文本颜色更改为深蓝色，使用十六进制代码值 #003078。

最后我们将处理 `:focus` 状态。焦点状态可用于链接、按钮和表单输入。焦点状态在用户点击或点按元素时显示。当元素获得焦点时，我们将向元素添加 1 像素的绯红色轮廓。

全部放在一起，我们的链接规则如下（列表 1.8）：

## Listing 1.9 Styling links using pseudo elements

```
a:link {
  color: #1D70B8;
}
a:visited {
  color: #4C2C92
}
a:hover {
  color: #003078;
}
a:focus {
  outline: solid 1px crimson;
}
```

我们可以通过进入浏览器，右键单击并选择“检查”来查看不同的元素状态。然后我们通常会看到 HTML 的视图，右侧为 CSS。通过点击样式部分中的 `:hov` 按钮，我们可以看到一个面板，可能显示“强制元素状态”，然后我们可以打开和关闭不同的伪类。图 1.20 显示了 Chrome 开发工具中打开的 :hov 面板。

