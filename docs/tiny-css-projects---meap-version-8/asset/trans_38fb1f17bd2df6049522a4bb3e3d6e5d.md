

Figure 1.24: 使用命名空间和通用选择器来设置 SVG 元素的背景

## 1.7 不同方式编写 CSS

CSS 在我们编写规则和格式方面具有灵活性。我们现在将查看简写属性（我们在整本书中都会反复提到）以及我们可以格式化 CSS 的不同方式。

### 1.7.1 简写

简写是一种通过将所有值合并到一个属性中来替代编写多个 CSS 属性的方法。我们可以用一些属性来做到这一点，例如 padding、margin、animation……这些我们将在本书的各个部分中介绍。编写简写的优点是我们可以减少样式表的大小，从而提高可读性、性能和内存使用。

每个简写属性接受不同的值。让我们探索我们在项目中使用的那个。我们在文章中有 `blockquote`。当我们对其进行样式设置时，我们使用了 `padding` 属性。我们将 padding 声明如下：`padding: 10px`。这样做时我们使用了简写。我们本可以改写如下（Listing 1.12）：

### Listing 1.15 Padding expanded

```
padding-top: 10px;
padding-right: 10px;
padding-bottom: 10px;
padding-left: 10px
```

完全可以单独编写每个声明，但就计算性能而言代价昂贵。尤其是当所有属性值都相同时更是如此。

相反，我们可以使用 `padding` 属性并将四个值放在同一行上。顺序为上、右、下、左，如 Figure 1.25 所描述。

![](img/b56145aa9b751926a7aedbd7172bc818_40_0.png)

Figure 1.25 Padding shorthand property explained

如图所示，我们可以声明全部 4 个值来定义上、右、下、左的值。但如果我们说左右相同而上下不同，那么我们可以指定 3 个值，顺序为上、左右、下。

如果声明了 2 个值，则表示第一个值是上下应采用的值，然后第二个值设置左右。最后如果只声明了 1 个值，则设置全部 4 个边。

### 1.7.2 格式化

我们编写 CSS 有几种方式，通常在查看他人的代码时我们会看到不同的格式。以下是一些示例：

多行格式（Listing 1.13）很可能是格式化选择中最受欢迎的一种。每个声明都单独占一行，然后使用制表符或空格缩进。

### Listing 1.16 The multi-line format

```
h1 {
  color: red;
  font-size: 16px;
  font-family: sans-serif
}
```

这种多行格式的变体（Listing 1.14）将大括号放在单独一行。它是我们可能在 PHP 语言中看到的。将大括号放在单独一行可能被认为是不必要的。

### Listing 1.17 A variation of the multi-line format

```
h1
{
  color: red;
  font-size: 16px;
  font-family: sans-serif
}
```

单行格式（Listing 1.15）很有意义，它紧凑，我们可以快速浏览文件，知道第一部分是选择器。然而，缺点是如果规则包含许多声明，阅读起来很快就会变得困难。

### Listing 1.18 The single-line format

```
h1 { color: red; font-size: 16px; font-family: sans-serif }
```

所有选项都有利有弊，但我们会注意到本书中的项目使用了一种组合，即选项一和三。主要需要知道的是没有对错之分，通常取决于什么对你或你的团队最有效。只要代码易于理解，那就足够了。

有鹰眼的人也会注意到上面的示例中，最后规则的声明上没有分号（;）。是否添加它是可选的，但为了更好的网页性能，最好实践是将其移除。

CSS 最好的部分之一是我们可以用最舒适的方式编写它。

## 1.8 Summary

- CSS 是一种成熟的编码语言，CSS 的每个部分都由模块组成。
- 模块取代了 CSS3 等大型版本，不会有 CSS4 版本发布。
- 内联 CSS 可以获得最高优先级并且性能良好，然而它非常重复且难以维护。
- 外部 CSS 使我们的 CSS 与 HTML 分离，保持关注点分离。
- 除了我们自己的 CSS 之外，浏览器还会应用默认样式。
- 用户也可以应用自己的 CSS，这可能会覆盖作者和用户代理样式表。
- 使用 !important 被认为是不良实践。
- CSS 规则由选择器和一个或多个声明组成。
- 有许多类型的选择器，我们可以针对它们创建规则，每个选择器都有自己的特异性级别

## 2

## 使用 CSS 网格设计布局

### This chapter covers

- 探索网格轨道以及我们如何安排网格
- 在 CSS 网格中使用 minimax 和 repeat 函数
- 使用分数单位以及它在 CSS 网格中独特性
- 创建模板区域并将项目放置在区域上
- 使用网格时的可访问性考虑
- 在网格中创建列和行之间的间距

现在我们对 CSS 的工作原理有了基本了解，我们可以开始探索布局 HTML 内容的选项，包括网格、弹性盒和列。首先，我们将专注于使用网格进行布局。

网格，在这种意义上，是相互交叉形成一系列正方形或矩形的线网络。现在得到所有主要浏览器的支持，网格已成为非常流行的布局技术。

本质上，网格由列和行组成，就像表格一样。我们首先创建我们的网格，然后将项目或元素分配到该网格，就像我们在玩大富翁游戏时将船只放置在网格上一样。

尽管有时与表格概念相比，网格布局和表格有非常不同的用途并满足非常不同的需求。网格用于布局，而表格用于表格数据。如果正在设置样式的内容适合 Excel 表格，那么它就是表格数据，应该放在表格中。

历史上我们曾使用表格进行布局，有时仍然需要这样做（例如电子邮件仅支持 CSS 的子集），但在网络上这被认为是不良实践。我们现在可以使用网格代替。

CSS 网格使我们能够发挥创造力，并为不同条件下的布局制作一系列布局并与媒体查询结合进行适配，正如我们将看到的那样。我们将使用

grid 来设置我们的项目样式，到本章结束时，我们的布局将如下所示（Figure 2.1）：

![](img/b56145aa9b751926a7aedbd7172bc818_43_0.png)

Figure 2.1 Final output

我们从 Github 仓库（https://github.com/michaelgearon/Tiny-CSS-Projects）的 chapter-02 文件夹中找到的起始 HTML 如下所示：

### Listing 2.1 Project HTML

```
<body>
  <main> #A
    <header> #B
      <img src="./img/logo.jpg" alt="">
      <h1>Sonnets by William Shakespeare</h1>
    </header>
    <article> #B
      <h2>
        Sonnet 1
        <br><small>by William Shakespeare</small>
      </h2>
      <p>
        <span>From fairest creatures we desire increase,</span>
        ...
      </p>
    </article>
    <aside> #B
      <section aria-label="sonnet 2">
        <img src="./img/image-1.jpg" alt="">
        <h3>Sonnet 2</h3>
        <p>
          When forty winters shall besiege thy brow,
          <br>And dig deep trenches in thy beauty's field, ...
        </p>
      </section>
```

```
<a href="">Read more of Sonnet 2</a>
</section>
<section>
<img src="./img/image-2.jpg" alt="">
<h3>Sonnet 3</h3>
<p>
Look in thy glass and tell the face thou viewest,
<br>Now is the time that face should form another, ...
</p>
<a href="">Read more of Sonnet 3</a>
</section>
</aside>
<section class="author-details"> #B
<h3>
<small>About the Author</small>
<br>William Shakespeare
</h3>
<p>English playwright, poet, ...</p>
</section>
<section class="plays"> #B
<img src="./img/play.jpg" alt="">
<h3>Checkout out his plays:</h3>
<ul>
<li><a href="">All's Well That Ends Well</a></li>
...
</ul>
<h3></h3>
</section>
<footer> #B
<p>Want to read more ...</p>
</footer>
</main>
</body>
```

#A 项目的容器

#B 容器内的子项

我们还有一些起始 CSS（Listing 2.2）只是为了指导我们在开始将 HTML 元素放置到网格格式时。我们不会担心这些预设样式（例如 margin、padding、颜色、排版和边框），因为这些概念在本书的其他部分有介绍，我们特别希望本项目专注于布局。

我们将字体从衬线改为无衬线，并使用 margin 增加浏览器窗口边界与容器之间的间距。我们还将图像浮动到左侧并调整行高、排版和内边距。

请注意，我们为 main 元素的直接子元素添加了边框和一些内边距。这些是为了帮助我们定义布局。我们将在项目后期将其移除。

