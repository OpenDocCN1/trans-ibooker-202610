

# 注意事项

我们使用 GitHub 的模板功能来设置仓库，以便用户更容易创建自己的副本。如果你想复制别人的 GitHub 仓库却看不到模板按钮，请点击 Fork 按钮，这会以另一种方式创建副本。区别在于：模板允许你通过赋予不同名称来创建同一仓库的多个副本，而 Fork 只允许你创建一份副本，因为它使用与原仓库相同的名称，而 GitHub 阻止你创建两个同名仓库。如果你需要为 GitHub 仓库创建第二个 Fork，请前往“在 GitHub 上创建新仓库并上传文件”。

5. 点击 *index.html* 文件查看代码。如果这是你第一次查看计算机代码，可能会觉得 overwhelming，但放松！我们插入了几个“代码注释”来解释正在发生的事情。第一个代码块告诉 Web 浏览器对代码页的其余部分应用哪些格式。第二个代码块指示浏览器加载 **Leaflet** 代码库，即构建交互式地图的开源软件。第三个代码块描述地图和标题应如何定位在屏幕上。好消息是，你不需要触碰这些代码块，所以保持原样即可。但你需要修改几行。

![](img/216890131d46798dfe1cd5f886cc608c_279_0.png)

6. 要编辑代码，请点击右上角的铅笔符号（✎）。

让我们先做一个简单的更改来证明你现在正在编辑地图，通过修改地图标题，该标题出现在大约第 21 到 23 行的 HTML division 标签块中。

+   7. 在行 <div id="map-title">EDIT your map title</div> 中，将新地图标题输入到 EDIT your map title 的位置。请注意不要删除 < > 符号内两端的 HTML 标签。

+   8. 要保存你的编辑，请滚动到页面底部并点击绿色的“Commit changes”按钮。

# Commit changes

Update index.html

Add an optional extended description...

Commit directly to the master branch.

Create a new branch for this commit and start a pull request.

Commit changes
Cancel

在程序员的语言中，我们“提交”更改，就像大多数人“保存”文档一样，以后你会看到 GitHub 如何跟踪每次代码提交，以便在需要时回滚。默认情况下，GitHub 将提交的简短描述插入为“Update index.html”，当你开始大量提交以跟踪工作时，你可以选择自定义该描述。此外，GitHub 将你的更改直接提交到代码的默认分支，我们稍后会解释。

# 提示

如果你希望将代码存储在 GitHub 上，但需要扩展到更大规模的商业级 Web 主机，请查看诸如 Netlify 之类的 freemium 服务。Netlify 会自动检测你推送到 GitHub 仓库的任何更改并将其部署到你的在线站点。

现在让我们将你编辑的地图发布到公共网络上，看看它在 Web 浏览器中的显示效果。GitHub 不仅存储开源代码，其内置的 GitHub Pages 功能还允许你托管 HTML 为基础代码的实时在线版本，任何拥有 Web 地址的人都可以在其浏览器中查看。虽然 GitHub Pages 免费使用，但对其使用、文件大小和内容有一些限制，并且不适用于运营在线业务或商业交易。但代码模板的一个优势是，你可以将其托管在你控制的任何 Web 服务器上。由于我们已经使用 GitHub 来存储和编辑代码模板，因此打开 GitHub Pages 在线托管它非常容易。

1. 要访问 GitHub Pages，请滚动到仓库页面的顶部并点击 Settings。

2. 在 Settings 屏幕中，向下滚动到 GitHub Pages 区域。在下拉菜单中，将 Source 从 None 改为 Main，保持中间默认的 /(root) 选项不变，然后按 Save。此步骤告诉 GitHub 在公共网络上发布你地图的实时版本，任何拥有 Web 地址的人都可以在其浏览器中访问。

# GitHub Pages

GitHub Pages 旨在托管你的个人、组织

# Source

GitHub Pages 目前已禁用。请在下方选择源

Branch: main

/ (root)

Save

# Select branch

Select branch

main

None

# 提示

作为对 2020 年 Black Lives Matter 运动的回应，GitHub 将其默认分支从 master 重命名为 main，以消除其 master-slave 隐喻。

3. 向下滚动到 Settings > GitHub Pages 以查看你的实时地图已在线发布的 Web 地址，并右键单击它以在新浏览器标签页中打开。

# GitHub Pages

GitHub Pages 旨在从 GitHub 仓库托管你的个人、组织或项目页面。

![](img/216890131d46798dfe1cd5f886cc608c_282_0.png)

现在你应该在浏览器中至少打开了两个标签页。第一个标签页包含你的 GitHub 仓库，你在其中编辑代码，其 Web 地址格式如下。 将 USERNAME 和 REPOSITORY 替换为你自己的：

```
https://github.com/USERNAME/REPOSITORY
```

第二个标签页包含你的 GitHub Pages 实时网站，你的编辑代码在线显示。GitHub Pages 自动生成以下格式的公共 Web 地址：

```
https://USERNAME.github.io/REPOSITORY
```

# 注意事项

你的代码的实时版本默认指向 index.html 页面，因此在 Web 地址中包含它是不必要的。

还记得我们建议你不要使用像 DrunkBrownieChef6789 这样的用户名创建账户吗？GitHub 会自动将你的用户名放在公共 Web 地址中。

保持两个标签页打开，以便你可以在编辑代码和查看在线实时结果之间轻松切换。

# 提示

GitHub Pages 通常在 30 秒内显示你的实时地图，但在某些情况下可能需要几分钟。如果一分钟后仍未看到更改，请给浏览器做一次“硬刷新”以绕过缓存中保存的内容并重新从服务器下载整个网页，使用以下其中一种键组合：

+   - Ctrl+F5（适用于 Windows 或 Linux 的大多数浏览器）
+   - Command-Shift-R（适用于 Mac 的 Chrome 或 Firefox）
+   - Shift-Reload 按钮工具栏（适用于 Mac 的 Safari）
+   - Ctrl+Shift+Backspace（适用于 Chromebook）

现在让我们编辑 GitHub 仓库，以便链接指向 *你的* 实时地图，而不是 *我们的* 实时地图：

+   1. 从第二个浏览器标签页复制你的实时地图 Web 地址。
+   2. 返回到包含你 GitHub 仓库的第一个浏览器标签页，并点击仓库标题返回其主页。
+   3. 在你的仓库页面上，点击打开 README.md 文件，再次点击铅笔编辑它。将你的实时 Web 链接粘贴在标签 (*replace with link to your site*) 下方，然后向下滚动以提交更改。

![](img/216890131d46798dfe1cd5f886cc608c_283_0.png)

![](img/216890131d46798dfe1cd5f886cc608c_284_0.png)

# # leaflet-map-simple

一个简单的 leaflet 地图模板，供新用户 Fork 自己的副本、编辑并托管在 GitHub Pages 上

# ## Link to live map (replace with link to your site)

[https://handsondataviz.github.io/leaflet-map-simple/](https://handsondataviz.github.io/leaflet-map-simple/)

现在你已成功进行简单编辑并发布了实时地图，让我们进行更多编辑来让它更精彩，并帮助你了解更多关于 Leaflet 代码的工作原理。

+   4. 在你的仓库主页上，点击打开 `index.html` 文件，并点击铅笔符号编辑更多代码。

每当你看到 EDIT 代码注释时，这就指出了你可以轻松修改的一行。例如，查找下面设置地图初始中心点及其缩放级别的代码块。插入新的纬度和经度坐标以设置新的中心点。要查找坐标，请在 Google 地图上右键单击任意点并选择“What's here?”，如“在 Google 表格中地理编码地址”所述。

```
var map = L.map('map', {
    center: [41.77, -72.69], // EDIT coordinates to recenter map
    zoom: 12,  // EDIT from 1 (zoomed out) to 18 (zoomed in)
    scrollWheelZoom: false,
    tap: false
});
```

下一个代码块显示用作地图背景的基础地图图块层。我们的模板使用带有所有标签的浅色地图，由 CARTO 公开提供，并归功于 OpenStreetMap。一个简单的编辑是将 `light_all` 更改为 `dark_all`，这将替换为具有反转配色的不同 CARTO 底图。或者预览几个其他 Leaflet 底图代码选项供你复制粘贴。确保注明来源，并保留此代码块末尾的 `}) .addTo(map);`，以显示底图：

```
L.tileLayer(
    'https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
    attribution: '&copy; <a href="https://osm.org/copyright">\
OpenStreetMap</a> contributors, &copy;\
<a href="https://carto.com/attribution">CARTO</a>'
    }).addTo(map);
```

最后一个代码块在地图上显示一个单点标记，默认在 Leaflet 中为蓝色，并在用户点击时弹出消息。你可以编辑标记坐标、插入弹出文本，或复制粘贴代码块以创建第二个标记：

```
L.marker([41.77, -72.69]).addTo(map) // EDIT marker coordinates
    .bindPopup("Insert pop-up text here"); // EDIT pop-up text message
```

> **警告**
> 
> 编辑代码时要小心。意外删除或添加额外的标点符号（如引号、逗号或分号）可能会导致你的地图无法工作。但破坏代码并修复它也可以成为学习的好方法。

+   5. 进行编辑后，记得向下滚动并按 Commit 保存更改。然后转到浏览器中包含实时地图的标签页，并执行硬刷新以查看更改。对地图的编辑通常会在 30 秒内出现，但请记住 GitHub Pages 有时需要更长时间来处理代码提交。如果出现问题，请参阅附录。

恭喜！如果这是你第一次编辑计算机代码并将其托管在线，你现在可以称自己为“程序员”。这个过程类似于遵循和修改食谱，就像你也可以在烤出第一批布朗尼后称自己为“厨师”一样！虽然在这个早期阶段不太可能有人会聘请你作为全职付费程序员（或厨师），但你现在已经了解了复制、编辑和在线托管代码所需的基本技能之一，你已准备好深入研究更高级的版本，例如第 11 章中的 Chart.js 和 Highcharts 模板以及第 12 章中的 Leaflet 地图模板。

# 将 GitHub Pages 链接转换为 Iframe

在第 9 章中，我们讨论了从主站点显示交互式内容并使其无缝出现在次要站点的好处。你还学习了如何将非常长的 Datawrapper 和 Tableau Public 嵌入代码转换为较短的 iframe 标签，以便在需要时更轻松地将其嵌入到次要网站。

同样的概念也适用于 GitHub Pages。当你在 GitHub Pages 上发布图表或地图（或任何内容）的代码模板时，它会生成一个在线链接，你可以使用与上述相同的原则将其转换为 iframe 标签，以便将其嵌入到次要网站。按照以下步骤操作：

+   1. 对于你已在线发布的任何 GitHub 仓库，请转到其 *Settings* 页面并向下滚动以复制其 GitHub Pages Web 地址，该地址将以以下通用格式出现：

```
https://USERNAME.github.io/REPOSITORY
```

