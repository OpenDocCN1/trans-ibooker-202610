

- Sublime: https://www.sublimetext.com
- Brackets: https://brackets.io
- Visual Studio Code: https://code.visualstudio.com
- 基本文本编辑器，例如
  - TextEdit（Mac）
  - Notepad（Windows）
  - Gedit（Linux）

你也可以使用免费的在线开发编辑器，如 CodePen(https://codepen.io)。在线开发编辑器是测试想法和为前端项目提供快速便捷访问的绝佳方式。

一旦你在电脑上安装了代码编辑器或选择了在线编辑器并创建了账户，我们就需要获取本章的入门代码。

在 Github 上创建了一个代码仓库（https://github.com/michaelgearon/Tiny-CSS-Projects），其中包含你跟随每章所需的所有代码。图 1.2 显示了该仓库的截图。

![](img/b56145aa9b751926a7aedbd7172bc818_7_0.png)

代码按章节组织成文件夹。每个章节文件夹内包含 2 个版本的代码：

- before：包含项目的入门代码。这是如果你跟随章节编码时想要的版本
- after：已完成的项目，如本章末尾应用了所展示 CSS 时的状态

下载，（或如果你熟悉 git，可以克隆），项目选择屏幕顶部的“Code”下拉菜单。

假设你正在跟随章节编码，从 chapter-01 中的“before”文件夹获取文件并复制到你的项目文件夹或你的 pen 中。你应该会看到一个带有一些入门代码的 HTML 文件，以及一个空的 CSS 文件。如果你将 HTML 文件在网页浏览器中打开或将内容复制到 CodePen，你会看到内容目前未加样式，除了浏览器提供的默认样式外，如图 1.3 所示。我们现在准备好使用 CSS 开始设置内容样式。

## Listing 1.1 Starting HTML

```
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Chapter 1 - CSS introduction</title>
<link rel="stylesheet" href="styles.css">
</head>
<body>
<img src="sample-image.svg" width="100" alt="">
<article>
<header>
<h1>Title of our article (heading 1)</h1>
<p>
Posted on
<time datetime="2015-05-16 19:00">May 16</time>
by
Lisa.
</p>
</header>
<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque tincidunt dapibus eleifend. Nam eu urna ipsum. Etiam consequat ac dolor et dapibus. Duis eros arcu, interdum eu volutpat ac, lacinia a tortor. Vivamus justo tortor, porttitor in arcu nec, pretium viverra ipsum. Nam sit amet nibh magna. Sed ut imperdiet orci, id finibus justo. Maecenas magna mauris, tempor nec tempor id, aliquam et nibh. Nunc elementum ut purus id eleifend. Phasellus pulvinar dui orci, sed eleifend magna ullamcorper sit amet. Proin iaculis lacus congue aliquam sodales.</p>
<ol class="ordered-list">
<li>List item 1</li>
<ul>
<li>Nested item 1</li>
<li>Nested item 2</li>
</ul>
<li>List item 2</li>
<li>List item 3</li>
<li>List item 4</li>
</ol>
<img src="sample-image.svg" width="200" alt="">
<p>Curabitur id augue nulla. Aliquam purus urna, aliquam eu ornare id, maximus et tellus. Aliquam eleifend sem vitae
urna blandit, non bibendum tellus dignissim. Aliquam imperdiet imperdiet sapien sit amet consectetur. Nam
convallis turpis felis,
sed vulputate lacus eleifend a. Mauris pharetra imperdiet lacinia. Sed sit amet
feugiat lectus, in consectetur
magna. Vestibulum accumsan porta enim at ultricies. Vestibulum vitae massa quis massa
dignissim imperdiet.</p>
<blockquote id="quote-by-author">Nunc eleifend nulla lobortis porta rhoncus. Vivamus
feugiat, sem vitae feugiat
aliquam, orci nulla venenatis libero, vitae rhoncus nibh neque ac velit.</blockquote>
<p>Etiam tempor vulputate varius. Duis at metus ut eros ultrices facilisis. Donec ut
est finibus, egestas nisl eu,
placerat neque. Pellentesque cursus, turpis nec sollicitudin sodales, nisi tellus
ultrices lectus, nec facilisis
purus neque vitae
diam. Nunc eleifend nulla lobortis porta rhoncus. Vivamus feugiat, sem vitae feugiat
aliquam, orci nulla venenatis
libero, vitae rhoncus nibh neque ac velit. Donec non fringilla magna. Vivamus
eleifend ligula libero, fermentum
imperdiet arcu viverra
in. Vivamus pellentesque odio interdum mauris aliquam scelerisque.</p>
<h2>Heading 2</h2>
<p>In ac euismod tortor. Vivamus vitae velit efficitur, mattis turpis quis, tincidunt
elit.
<a target="_blank" href="#">In eleifend in dolor id aliquet</a>. Vivamus pellentesque
erat a magna ultricies
rhoncus. Vestibulum at mattis purus, non lobortis risus. Mauris porta ullamcorper
mollis. Sed et placerat nisi,
quis porttitor lacus.
Curabitur sagittis nisl egestas ipsum tristique, eu semper erat gravida. Vestibulum
sagittis quam sit amet
tristique ultricies.
</p>
<p>In id lobortis leo. Nullam commodo tortor eu neque tempus accumsan. Vivamus
molestie, felis consequat consequat
iaculis, justo massa porttitor tellus, ac suscipit urna erat eu erat. Nunc malesuada
eleifend erat nec pharetra.
Sed eu magna iaculis,
elementum dui ac, sagittis augue. Nam sit amet risus dapibus massa rutrum faucibus.
Sed rhoncus finibus magna, vel
tristique sem bibendum nec.</p>
<img src="sample-image.svg" width="200" alt="">
<h3>Heading 3</h3>
<p>Mauris sit amet tempor ex. Morbi eu semper velit. Nullam hendrerit urna
pellentesque, interdum lectus volutpat,
gravida odio.
<a href="#">Sed vulputate eget ante vel vehicula</a>. Curabitur ac velit sed magna
malesuada hendrerit. Vestibulum
ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia curae; Ut
volpat nisi purus. Morbi
venenatis fermentum commodo.
Nam accumsan mollis neque non interdum. Aenean cursus metus ac est gravida, placerat
interdum justo pellentesque.
Duis nec scelerisque lacus, elementum tincidunt est. Maecenas et leo justo. Nam porta
risus porttitor vulputate
laoreet. Nulla sodales
sagittis nulla, non viverra erat consectetur et.
</p>
<p>Quisque vel erat et leo efficitur sollicitudin. Donec id ipsum eget lacus
pellentesque consequat. Praesent id
justo tellus. Maecenas convallis magna ut diam euismod, et porta lacus lacinia. Sed a
placerat justo. Quisque
dictum augue quis dapibus
posuere. Sed rutrum sed leo eget pretium. Donec fermentum varius vestibulum. Nam
ultrices ornare risus, vel
malesuada sapien volutpat ac. Duis sit amet leo ut metus dignissim sollicitudin nec
eget diam. Nullam tortor erat,
tincidunt ut venenatis at,
lacinia in libero. Mauris feugiat commodo lectus eget hendrerit. Curabitur pharetra
cursus eleifend.</p>
<h4 class="small-heading">Heading 4</h4>
<p>Aliquam porttitor, ex sed suscipit scelerisque, arcu urna rhoncus lacus, eget
aliquam enim quam ut lacus.
<a href="#">Cras sed finibus libero</a>. Duis lobortis, ipsum ut consectetur
eleifend, libero magna ullamcorper
ex, eget eleifend mi risus in erat. Ut consequat nunc diam, et convallis orci
efficitur id. Etiam finibus metus a
bibendum lobortis. Sed
est turpis, maximus id magna vitae, lobortis faucibus turpis. Maecenas in ipsum ut
nibh dignissim venenatis.
Aenean laoreet, arcu a ornare feugiat, augue enim bibendum leo, in dapibus tortor
ipsum nec massa. Pellentesque
mollis massa magna, imperdiet
elementum justo sodales vitae.
</p>
<h2>Heading 2</h2>
<h3>Heading 3</h3>
<svg xmlns="http://www.w3.org/">
<circle cx="70" cy="70" r="50"></circle>
<rect y="80" x="200" width="50" height="50" />
</svg>
<h4>Heading 4</h4>
<h5 class="small-heading">Heading 5</h5>
<p>In finibus ultrices nulla ut rhoncus. Suspendisse potenti. Phasellus id tortor nec
elit aliquet ullamcorper ut ut
tortor. Morbi vehicula massa sit amet luctus posuere. Nullam eu lacinia tortor.
Aenean eget pulvinar nulla, sit
amet accumsan tortor.
Praesent augue nunc, luctus ornare consequat id, ultricies vel tellus. Nullam gravida
tellus a nisi elementum
interdum. In scelerisque turpis vitae sem convallis venenatis. Donec aliquam, nibh
sit amet placerat posuere,
neque metus ultricies risus,
at luctus leo arcu a augue. Class aptent taciti sociosqu ad litora torquent per
conubia nostra, per inceptos
himenaeos. Sed congue lectus quis velit fringilla, a ultricies dui vestibulum.
Vestibulum ornare leo augue, vel
pretium ipsum vehicula et.
Nulla vel efficitur risus. Proin malesuada lobortis orci sit amet scelerisque. Sed
tincidunt felis vel aliquet
tristique.</p>
<h6 class="small-heading">Heading 6</h6>
<p lang="it">Questo paragrafo è definito in italiano.</p>
<ul class="list">
<li>List item 1</li>
<ul>
<li>Nested item 1</li>
<li>Nested item 2</li>
</ul>
```
