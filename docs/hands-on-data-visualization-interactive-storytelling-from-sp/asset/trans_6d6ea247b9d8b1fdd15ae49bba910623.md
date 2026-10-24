
![](img/216890131d46798dfe1cd5f886cc608c_466_0.png)

鉴于此，在前面描述的两个 Leaflet 代码模板中，媒体文件路径名是区分大小写的。换句话说，`media/filename.jpg` 与 `media/filename.JPG` 并不相同。因此，我们建议全部使用小写字符，包括扩展名。

最后，在使用调用 GeoJSON 数据文件的 Leaflet 代码模板时（如第 12 章所述），请注意地理数据中的 null（空）字段错误。在上一节所述的浏览器控制台诊断窗口中，这些错误可能会显示类似于以下的 NaN（意为“非数字”）错误消息：

```
Uncaught Error: Invalid LatLng object: (NaN, NaN)
```

要解决浏览器控制台中的 NaN 错误，请使用“使用 GeoJson.io 绘制和编辑”中的 GeoJson.io 工具来仔细检查你的地理数据中是否存在空字段。

## 常见的 iframe 错误

如果你按照第 9 章的步骤操作，但 iframe 的内容仍未出现在浏览器中，请检查以下常见问题：

- iframe 中列出的项目（如 URL、宽度或高度）应包含在直单引号（'）或直双引号（"）之内。选择其中一种，但要保持一致。
- 始终使用直引号，避免使用弯引号（也称为智能引号或斜引号）。从文字处理器粘贴代码时，有时会意外发生这种情况。避免使用弯引号，例如左单引号（‘）、右单引号（’）、左双引号（“）和右双引号（”）。
- 在 iframe 中始终使用 https（额外的“s”表示“安全”），而不是 http。如果混合使用 https 和 http 资源，某些 Web 浏览器会阻止内容。本书中的所有代码模板都需要 https。

使用 W3Schools 的 TryIt iframe 页面来测试你的 iframe 嵌入代码，尤其是在需要编辑它们的时候，因为这是检查标点错误的好方法。图 A-2 显示了一个简单 iframe 中的三个常见问题：弯双引号（在 src= 之后）、使用 http 而不是 https，以及双引号和单引号混用。所有这些问题都在图 A-3 中得到了修正，这使得 iframe 能够按预期显示。

```
<!DOCTYPE html>
<html>
<body>
<h1>The iframe element</h1>
<iframe src="http://handsondataviz.github.io/leaflet-map-simple" width='90%' height='350'>
</iframe>
</body>
</html>
```

### iframe 元素

图 A-2. 你能在这个错误的 iframe 代码中找出三个常见问题吗？

```
<!DOCTYPE html>
<html>
<body>
<h1>The iframe element</h1>
<iframe src='https://handsondataviz.github.io/leaflet-map-simple' width='90%' height='350'>
</iframe>
</body>
</html>
```

### iframe 元素

图 A-3. 这里修正了所有三个问题，使得 iframe 能够按预期显示。

## 在 GitHub 上修复代码

正如我们在第 10 章中讨论的，使用开源代码模板可以让你更好地控制数据可视化的外观及其在线托管位置。这也意味着，当你的代码出错时，你需要负责修复它，或者找一个有能力的人帮你修复，可能需要付费。如果你在修复代码或在免费的 GitHub 平台上托管代码时遇到问题，请回顾本书的相关章节，并注意以下常见问题：

- 编辑代码时要小心。一个单独的拼写错误——例如缺少逗号、分号、引号或括号——都可能导致你的可视化出错。我们理解这种情况发生时你可能会感到多么沮丧，因为我们也经历过，所以稍作休息，过一会儿再回到屏幕前，用全新的眼光来帮助你找到问题。
- 耐心一点。GitHub Pages 通常会在 30 秒内处理你对可视化的编辑，但有时可能需要几分钟。给浏览器做一次“硬刷新”，以绕过缓存中保存的任何内容，并从服务器重新下载整个网页，请使用以下按键组合之一：
  - Ctrl+F5（大多数 Windows 或 Linux 浏览器）
  - Shift+Ctrl+R（Chromebook）
  - Command-Shift-R（Mac 上的 Chrome 或 Firefox）
  - Option-Command-R（Mac 上的 Safari）
- 始终在不同的浏览器中测试已发布可视化的链接。有时问题实际上是由浏览器故障引起的，而不是代码本身。
- 偶尔，GitHub 平台可能会遇到中断，或报告从你的代码构建 GitHub Pages 时出现已知问题。请查看 GitHub 状态站点。

在第 11 章使用 Chart.js 和 Highcharts 代码模板以及在第 12 章使用 Leaflet 地图代码模板时，要谨慎编辑，尤其是数据文件的结构。例如，在“Leaflet Maps with Google Sheets”代码模板中，除非你清楚自己在做什么，否则不要更改每列顶部的名称，因为代码模板会通过这些确切名称来处理你的数据：

![](img/216890131d46798dfe1cd5f886cc608c_468_0.png)

请记住，你可以更高效地在本地计算机上编辑和测试代码模板，而不是将每次更改都上传到 GitHub 在线查看。使用“使用 GitHub Desktop 和 Atom 文本编辑器高效编码”中所述的 GitHub Desktop 和 Atom 工具。要在本地计算机上完整查看更复杂的 Chart.js、Highcharts 或 Leaflet 代码模板，你可能需要按如图 10-2 和图 10-3 所示，临时管理浏览器中的 CORS 互联网安全设置。

随着时间的推移，代码模板需要维护，以确保它们随着技术的发展继续工作。例如，本书中介绍的代码模板都有代码依赖项，这意味着它们依赖其他代码或在线服务才能运行。这些依赖项包括创建图表和地图的在线代码库，例如 Chart.js、Highcharts、Leaflet 等。此外，地图代码模板依赖来自 CARTO、Stamen 和 OpenStreetMap 等提供商的在线地图瓦片。如果你的某个在线代码依赖项不再运行，你的代码模板很可能会停止工作。

要检查你的代码模板是否与其某个在线代码依赖项有关的问题，请返回你进行复制操作时所用的原始 GitHub 仓库。检查当前的在线演示图表或地图是否正常工作。如果是，那么检查原始 GitHub 仓库是否有最近可能解决你问题的代码更新。有些代码更新很简单，可以直接通过 GitHub 网页界面输入到你的仓库中。其他代码更新则更为复杂，因此请回顾如何使用诸如 GitHub Desktop 之类的工具将代码从仓库拉取到本地计算机，参见“使用 GitHub Desktop 和 Atom 文本编辑器高效编码”。

如果你复制代码模板的原始 GitHub 仓库的在线演示版本无法正常工作，请联系开源软件开发者；最好的方式是在他们的 GitHub 仓库上创建一个 Issue。无法保证开源软件开发者未来会继续维护他们的代码项目。但开源代码的一个优点是，任何人都可以创建新的 fork 副本，并自行维护，或与开源编码社区中的协作者一起维护。

最后，如果你没有在这里找到问题的答案，可以考虑在其他地方提问。我们推荐的一些工具支持页面包含指向社区帮助论坛的链接，用户可以在那里发布问题，有时会从其他用户那里获得有用的回答。此外，Stack Exchange 网络汇集了 170 多个在线社区，专家们可以就特定主题回答问题，例如面向 Google Sheets 等在线工具的 Web Applications Stack Exchange，以及面向软件编码的 Stack Overflow。在任何这些公共论坛上发帖时，请务必遵守它们的指南，清楚地描述你的问题，并提及你所使用的计算机操作系统类型和/或浏览器版本。

> 1 Jay Green，“Amazon Web Services 中断阻碍企业发展”，《华盛顿邮报》，商业版，2020 年 11 月 25 日。https://oreil.ly/PTmQ6。
