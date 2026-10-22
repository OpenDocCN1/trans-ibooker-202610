

![](img/b56145aa9b751926a7aedbd7172bc818_23_0.png)

Figure 1.10 CSS order of precedence

我们已经确定选择器的类型会影响类的特异性。让我们更仔细地查看选择器并在项目中使用它们。

## 1.6 CSS 选择器

选择器设置我们想要定位的 HTML 元素，然后跟随声明。

在 CSS 中，有七种方法可以定位我们想要设置样式的 HTML 元素。让我们先来看基本选择器。

### 1.6.1 基本选择器

最常见的方法是根据名称、ID 或类名选择元素。

### TYPE SELECTOR

类型选择器通过名称定位 HTML 元素。使用类型选择器的好处是，在阅读 CSS 时，我们可以快速判断如果修改规则会受到影响的 HTML 元素。它也不要求我们在 HTML 中添加任何特定标记来定位元素。

让我们使用类型选择器来定位我们所有的标题（h1 到 h6），并将它们的颜色更改为深红色。

我们的 CSS 将是：h1, h2, h3, h4, h5, h6 { color: crimson; }。图 1.11 显示我们所有的标题颜色现在都已更改。

# Figure 1.11 Header color change

### CLASS SELECTORS

类选择器可以应用于我们想要的任意多个不同元素。通过将类名应用于元素，我们将多个 HTML 元素分组在一起，这样当我们应用样式时，它们将应用到任何具有该类名的元素。

类可以通过使用 class 属性添加到 HTML 中。在 class 属性内，我们可以添加任意多个值（或类），每个值之间用空格分隔。与必须用美式英语书写的声明不同，类名可以用任何语言使用任何拼写变体书写。

编写类名有很多方法和方式，例如 Block、Element、Modifier（BEM）方法或 CSS 的可扩展模块化架构（SMACSS），它们是编写一致样式表的风格指南。主要目的是编写任何人都能理解的类名。

例如，将类名 "text" 添加到我们的段落元素会非常令人困惑。这是因为其他元素，如我们的标题，也可以被视为文本，因此可能不清楚我们具体指的是哪个元素。基于特定样式（如颜色）应用类名也可能很危险。给元素一个名为 blue 的类名可能立即有效，但如果设计更改且应用的颜色现在是红色，我们的类名将不再有意义。

在我们的 HTML 中，我们发现一些标题具有 small-heading 的类。我们将创建一个规则来选择 small-heading 并将元素的文本更改为大写。

要选择 small-heading 类名，在 CSS 中我们将首先输入点号（.），后跟类名 small-heading，然后我们的样式进入大括号，与类选择器相同：.small-heading { text-transform: uppercase }。图 1.12 现在显示了我们的大写标题。请注意，其他标题未受影响。只有应用了该类的那些。

Quisque dictum augue quis dapibus posuere. Sed rutrum sed leo eget pretium. Donec fermentum varius vestibulum. Nam ultrices ornare dignissim sollicitudin nec eget diam. Nullam tortor erat, tincidunt ut venenatis at, lacinia in libero. Mauris feugiat commodo lectus eget

# HEADING 4

# Has a class of small-heading

Aliquam porttitor, ex sed suscipit scelerisque, arcu urna rhoncus lacus, eget aliquam enim quam ut lacus. Cras sed finibus libero. Duis lo eleifend mi risus in erat. Ut consequat nunc diam, et convallis orci efficitur id. Etiam finibus metus a bibendum lobortis. Sed est turpis, nibh dignissim venenatis. Aenean laoreet, arcu a ornare feugiat, augue enim bibendum leo, in dapibus tortor ipsum nec massa. Pellentes

# Heading 2

# Heading 3

![](img/b56145aa9b751926a7aedbd7172bc818_26_0.png)

![](img/b56145aa9b751926a7aedbd7172bc818_26_1.png)

# Heading 4

# Does not have a class of small-heading

# HEADING 5

In finibus ultrices nulla ut rhoncus. Suspendisse potenti. Phasellus id tortor nec elit aliquet ullamcorper ut ut tortor. Morbi vehicula mass nulla, sit amet accumsan tortor. Praesent augue nunc, luctus ornare consequat id, ultricies vel tellus. Nullam gravida tellus a nisi element aliquam, nibh sit amet placerat posuere, neque metus ultricies risus, at luctus leo arcu a augue. Class aptent taciti sociosqu ad litora torqu velit fringilla, a ultricies dui vestibulum. Vestibulum ornare leo augue, vel pretium ipsum vehicula et. Nulla vel efficitur risus. Proin mal tristique.

# HEADING 6

Questo paragrafo è definito in italiano.

- List item 1

Figure 1.12 Class-selector applied to heading 4, 5 and 6

### Id SELECTOR

在 HTML 中，id 是唯一的。任何给定的 id 在网页上只能使用一次。如果重复使用多次，则我们的代码被视为无效 HTML。通常我们应避免使用 id 选择器，因为由于 id 在 HTML 中需要唯一，针对 id 构建的类不易重用。此外，id 选择器是最具体的选择器之一，使得使用 id 应用的样式难以覆盖。除非元素的唯一性是关键，否则应避免使用 id 属性。像类名一样，id 名称也可以用任何语言书写。

在我们的示例文章中，有一个带有 id 属性值为 quote-by-author 的 blockquote。在我们的 CSS 中，要选择该 blockquote，我们使用井号（#）紧跟其后是要定位的 id。然后我们有大括号，在其中放置声明，如 Listing 1.4 所示。

# Listing 1.5 shows our rule.

```
#quote-by-author {
  background: lightgrey;
  padding: 10px;
  line-height: 1.75;
}
```

Figure 1.13 shows the code applied to our project.

![](img/b56145aa9b751926a7aedbd7172bc818_27_0.png)

Figure 1.13 shows the output

### 1.6.2 Combinators

另一种编写 CSS 的方式是通过组合器，它允许在不过度使用 `class` 或 `id` 名称的情况下进行更复杂的 CSS。

共有四种组合器：
- 1. 后代组合器（空格）
- 2. 子组合器（>） 
- 3. 相邻兄弟组合器（+） 
- 4. 通用兄弟组合器（~）

需要理解的一个重要概念是元素之间的关系。在接下来的几个示例中，我们将看看如何利用元素之间的关系来定位不同的 HTML 元素以设置文章的样式。图 1.14 介绍了我们将要查看的关系类型。

# Figure 1.14 The relationship between elements in HTML

### 1. DESCENDANT COMBINATOR (SPACE)

后代组合器选择父元素内的所有 HTML 元素。后代组合器由三部分组成——第一部分是父元素，在本例中是 article 元素。父元素后跟一个空格，然后是我们想要选择的元素。语法在图 1.15 中说明。

# A space between the parent and child

parent
Child items of the parent

```
article p {
  line-height: 2;
}
```

# Figure 1.15 An example of a descendant combinator

![](img/b56145aa9b751926a7aedbd7172bc818_28_0.png)

在此示例中，浏览器会找到 article 元素的任何使用情况，然后定位父 article 元素中的所有后代段落（p）并使文本双倍行距。应用后，我们的文章看起来如 Figure 1.16 所示。

![](img/b56145aa9b751926a7aedbd7172bc818_29_0.png)

Figure 1.16 Child paragraphs are double spaced.

### 2. CHILD COMBINATORS (>)

子组合器是父元素的子元素。这与后代组合器不同，因为目标元素必须是直接子元素。后代组合器可以选中任何后代（子元素、孙元素、曾孙元素...）。

在我们的项目中，我们将设置文章中列表项的样式。正如我们在下面的 HTML 中看到的（Listing 1.4），我们有一个无序列表（ul）包含列表项（li）。第一个子元素有自己的嵌套项，它们将是孙元素和曾孙元素。

# Listing 1.7 HTML list items

```
<ul class="list">      #A
  <li>List item 1</li>      #B
  <ul>                    #C
    <li>Nested item 1</li> #D
    <li>Nested item 2</li> #D
  </ul>
  <li>List item 2</li>    #B
  <li>List item 3</li>    #B
  <li>List item 4</li>    #B
</ul>                     #A
```
