

## 6
## 创建个人资料卡片
## 本章涵盖
- CSS 自定义属性
- 使用 radial-gradient 创建背景
- 设置图像大小
- 使用 flexbox 定位元素

在本章中我们将创建一个个人资料卡片。在网页设计中，卡片是一种包含单一主题信息的视觉元素。我们将把这个概念应用于某人的个人资料信息，本质上创建了一张数字名片。这种布局在社交媒体或博客网站上经常可以看到，以便读者快速了解内容的作者。它有时会链接到详细的个人资料页面或与该个人资料所属人互动的机会。

到本章结束时，我们的个人资料卡片将如下所示（图 6.1）：

![](img/b56145aa9b751926a7aedbd7172bc818_158_0.png)

图 6.1 最终输出

要创建此布局，我们将围绕定位做大量工作，特别是利用 flexbox 来帮助我们对齐和居中元素。我们还将了解如何使矩形图像适应圆形而不失真。

让我们直接开始并查看我们的起始 HTML（列表 6.1），它可以在 github 仓库 https://github.com/michaelgearon/Tiny-CSS-Projects/tree/main/chapter-06 中找到。

我们有一个 class 为 card 的 div，其中包含个人资料卡片中展示的所有元素。为了设置我们的博客文章信息，我们使用描述列表。

> **描述列表**
> 描述列表包含术语组，包括描述术语（dt）和任意数量的描述（dd）。描述列表通常用于词汇表或显示元数据。由于我们将术语（帖子、点赞和关注者）与其计数（数字）配对，因此使用描述列表是一个很好的用例。

我们的技术（CSS、HTML...）以列表形式呈现。

### 列表 6.1 项目 HTML

```
<body>
  <div class="card">  #A
    <img class="portrait" src="./img/portrait.jpg" alt=""> #B
    <h1>Annabelle Erickson</h1> #C
    <div class="title">Software Developer</div> #D
    <dl>          #E
      <div>        #E
        <dt>Posts</dt>      #E
        <dd>856</dd>       #E
      </div>        #E
      <div>        #E
        <dt>Likes</dt>      #E
        <dd>1358</dd>       #E
      </div>        #E
      <div>        #E
        <dt>Followers</dt>  #E
        <dd>1257</dd>       #E
      </div>        #E
    </dl>          #E
    <p class="summary">      #F
      I specialize in UX / UI design and development. I love coffee, chocolate, and my
      betta fish. #F
    </p>      #F
    <ul class="technologies">    #G
      <li>CSS</li>          #G
      <li>HTML</li>         #G
      <li>JavaScript</li>   #G
      <li>Accessibility</li> #G
    </ul>          #G
    <div class="actions">      #H
      <button type="button" class="follow">Follow</button>  #H
      <a href="" class="message">Message</a>          #H
    </div>          #H
  </div> #I
</body>
```

#A 卡片开始
#B 个人资料图片
#C 个人资料持有者姓名
#D 个人资料持有者职位
#E 帖子信息
#F 个人简介 / 关于
#G 技术
#H 操作

当我们开始为卡片设置样式时，我们的页面如下所示（图 6.2）。

![](img/b56145aa9b751926a7aedbd7172bc818_160_0.png)

图 6.2 起始点

### 6.1 CSS 自定义属性

在我们的布局中，特别是当我们为个人资料图片和卡片顶部图片下方的彩色部分设置样式时，我们需要多次使用图像大小值进行计算。在像 JavaScript 这样的其他语言中，当我们有一个需要多次引用的值时，我们使用变量。在 CSS 中我们有自定义属性，有时也称为 CSS 变量。

要创建自定义属性，我们将变量名以 2 个破折号（--）为前缀，紧跟变量名。我们以与任何其他属性相同的方式分配值，使用冒号（:）后跟值。因此 CSS 变量声明看起来如下：`--myVariableName: myValue;`

像任何其他声明一样，我们需要在规则内部定义我们的变量。对于我们的项目，我们将定义颜色和图像大小并在 body 规则中声明它们，如列表 6.2 所示。由于我们在 body 上定义了变量，body 元素及其所有后代都将访问这些变量。

请注意，我们可以为变量分配不同类型的值。我们分配颜色，例如在我们的 --primary 变量中（可能是 CSS 自定义属性最常见的用途），但我们也定义了大小（--imageSize）、字体族和渐变（--page-background）。

要引用变量并将其用作声明的一部分，我们使用以下语法：`var(--variableName)`，因此例如要分配我们的文本颜色，将声明：`color: var(--text-color);`

### 列表 6.2 定义 CSS 自定义属性

```
body {
  --primary: #de3c4b; #A
  --primary-contrast: white;
  --secondary: #717777; #B
  --font: Helvetica, Arial, sans-serif;
  --text-color: #2D3142; #C
  --card-background: white;
  --page-background: linear-gradient(#4F5D75, #2D3142);
  --imageSize: 200px;

  background: var(--page-background);
  font-family: var(--font);
  color: var(--text-color);
}
```

#A 深鲑鱼色
#B 灰色
#C 深蓝灰色

注意：线性渐变 我们的线性渐变将从上到下淡化，从深蓝色渐变为非常深的蓝色。有关线性渐变的更深入解释，请查看第 3 章第 3.7.2 节。

在应用了背景和字体颜色及族之后（图 6.3），我们注意到我们的背景在页面底部重复。

![](img/b56145aa9b751926a7aedbd7172bc818_161_0.png)

图 6.3 为 body 添加背景

### 6.2 全高背景

线性渐变是一种图像类型。当我们在 CSS 中将图像作为元素的背景应用时，如果图像小于元素，图像将重复或平铺。在这种特定情况下，我们不希望图像重复。

我们有 2 种方法可以修复这个问题：

- 我们可以告诉背景我们不希望它重复，使用 `background-repeat: no-repeat;` 然而由于我们的 body 元素仅与内容一样高，如果窗口比内容更高，我们将在页面底部留下不雅观的白色条，这并不理想。
- 我们的第二个选项（我们将使用的）是使 html 和 body 都占据屏幕的完整高度，而不是让其根据内部内容调整大小。

因此我们将列表 6.3 中的规则添加到我们的样式表中。

我们将 margin 和 padding 重置为 0，因为我们希望确保在窗口内边缘到边缘。

要设置高度，我们实际上使用 `min-height`，因为如果内容长度大于窗口高度，我们仍然希望用户能够访问内容并且背景在其后面。通过使用 `min-height`，我们指示浏览器使 `body` 和 `html` 元素至少与窗口一样高。如果内容会迫使元素更高，则浏览器将使用内容的高度。

我们为 `min-height` 设置的值是 `100vh`。视口高度（vh）是一个基于视口本身高度的单位。它基于百分比，因此将 `100vh` 分配给 `min-height` 意味着我们希望元素至少具有等于视口高度 100% 的高度。

### 列表 6.3 使背景全高

```
html, body {
  margin: 0;
  padding: 0;
  min-height: 100vh;
}
```

现在我们已经设置了背景（图 6.4），让我们为卡片设置样式。

![](img/b56145aa9b751926a7aedbd7172bc818_163_0.png)

图 6.4 全屏渐变背景

### 6.3 使用 flexbox 设置卡片样式并居中

让我们从为卡片本身设置样式开始。我们将为其提供白色背景和阴影，以为我们的布局增添深度。请注意，我们没有使用颜色值作为背景，而是使用我们的背景变量。

我们还将把卡片的宽度设置为 75vw。视口宽度（vw）是我们在前面使用的视口高度（vh）单位的水平对应物。它也是基于百分比的，因此将我们的宽度设置为 75vw 意味着将卡片的宽度设置为浏览器窗口总宽度的 75%。

然后我们将进一步将卡片的宽度限制为最大 500 像素宽。通过同时使用 width 和 max-width 属性，我们允许卡片在屏幕较窄时缩小，但在较大屏幕上限制卡片变得过于宽大。

最后我们使用 border radius 弯曲卡片的角落以软化设计。列表 6.4 显示了我们的卡片规则。

### 列表 6.4 设置卡片样式

```
.card {
  background-color: var(--card-background);
  box-shadow: 0px 0px 55px rgba(38, 40, 45, .75);
  width: 75vw;
  max-width: 500px;
  border-radius: 4px;
}
```

