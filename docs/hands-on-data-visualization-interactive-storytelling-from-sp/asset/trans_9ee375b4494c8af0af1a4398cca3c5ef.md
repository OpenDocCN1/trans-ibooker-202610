

此时，你应该对地图设计的[content]
此时，你应该对地图设计的关键原则和最佳实践有了更好的认识。现在我们已经介绍了交互式地图的一般关键概念，以及特别是等值分区地图的概念，接下来我们将转向一系列使用推荐工具的动手教程。在“使用 Google My Maps 的点地图”中，我们将使用 Google My Maps 创建带有自定义图标的点地图，以弹出窗口显示特定位置的信息。在“使用 Datawrapper 的符号点地图”中，我们将在 Datawrapper 中构建符号点地图，使用不同大小的有色圆圈来表示特定城市的人口变化。本章的最后两个教程将回到等值分区地图设计的主题，分别是“使用 Datawrapper 的等值分区地图”和“使用 Tableau Public 的等值分区地图”，以比较这两种工具。

## 使用 Google My Maps 的点地图

大多数人已经熟悉 Google Maps，这项网络地图服务允许用户查找世界各地的地点和路线。在本节中，你将了解 Google My Maps，这是一款相关工具，允许你在 Google Maps 平台上显示一组点，用户可以点击这些点以显示更多数据，包括照片、网站或路线。你可以自定义点标记的颜色和图标，你创建的所有地图图层内容都将保存在你的 Google Drive 中，在那里你可以编辑并与其他人协作。尽管 Google My Maps 功能有限，但它是一款易于学习的工具，可用于构建基本的交互式点地图，如果需要，还可包含简单的折线和多边形。最后，你可以分享地图的公共链接或将其嵌入到你的网站中，相关步骤将在第 9 章中详细介绍。

在本节中，我们将构建北美博物馆和公园的点地图，其中包含两种不同样式的标记组和一个自定义照片图标。当用户点击标记时，弹出窗口中会出现额外的文本、链接和图像，如图 7-15 所示。

## 示例博物馆和 ...

北美热门景点

17 次浏览

分享

## 博物馆和公园

- Museum
- Park
- Custom icon demo

![](img/216890131d46798dfe1cd5f886cc608c_192_0.png)

图 7-15. 使用 Google My Maps 创建的公园和博物馆点地图：探索交互式版本。

要创建自己的带自定义图标的交互式点地图，请按照本教程操作：

- 1. 打开 Google Sheets 中的 Parks and Museums 数据，其中包含北美六个热门地点。每行包含 Group、Name、Address 和 URL。登录你的 Google 账户并前往 File > "Make a copy"，以创建可在你的 Google Drive 中编辑的版本。
- 2. 导航到 Google My Maps。在左上角，点击 "Create a New Map" 按钮。这将创建一个具有熟悉 Google Maps 样式的空白地图。
- 3. 通过点击当前标题 Untitled map 并输入新的 Map title 和 Description 来添加相关标题和描述。
- 4. 要向地图添加数据，请点击 "Untitled layer" 项目下的 Import。

![](img/216890131d46798dfe1cd5f886cc608c_193_0.png)

5. 在 "Choose a file to import" 屏幕中，有几种上传数据的方式。选择 Google Drive，因为我们的示例数据已采用该格式，并选择 Recent 选项卡以找到你保存到 Google Drive 的 Museums and Parks 文件。按下 Select。

## 选择要导入的文件

我的云端硬盘
与我共享
最近

| 名称 | 所有者 | 最后修改时间 |
|---|---|---|
| Museums and Parks (go to File > Make a Copy) | 我 | 上午 11:09 |

6. 在 “Choose columns to position your placemarks” 屏幕中，选择 Address 列以将点数据放置在地图上。点击 Continue。

## 示例博物馆和公园

北美热门景点

15 次浏览

所有更改已保存在云端硬盘

添加图层

统一样式

数字序列

单独样式

按数据列样式化：

Group

Name

Address

URL

10. 在 “Styled by Group” 下，将光标悬停在 Museum 标签上以显示桶样式符号，然后点击它。

## 添加图层

## 分享

## 预览

## 博物馆和公园

## 按组样式化

Museum (3)

Park (3)

11. 为 Museums 分配新的颜色，并点击 “More icons” 以找到更合适的点标记符号。

## 添加图层

## 分享

## 预览

## 博物馆和公园

## 类别：Museum (3 项)

Museum

## 颜色

## 热门图标

## 更多图标

基础

12. 在 “Choose an icon” 屏幕中，使用右上角的 Filter 字段按名称搜索图标类型，例如 “museum”。对 “parks” 重复此过程。

## 选择图标

形状 体育和休闲 地点 交通 危机 天气 动物 自定义图标 Filter: museum

## 形状

## 体育和休闲

## 地点

13. 在 “Choose an icon” 屏幕中，你可以点击左下角的 “Custom icon” 按钮上传图像，该图像将被转换为缩略图图像图标。此自定义图标由华盛顿纪念碑的 Wikimedia 图像创建。

## 自定义图标

自定义图标 OK

14. 点击任意地图标记以编辑其数据，将照片插入其弹出窗口，或添加 Google 地图路线。此照片来自大都会艺术博物馆的 Wikimedia 图像。然而，你必须手动添加照片或路线，因为这些链接无法预先加载到数据电子表格中。

## 大都会艺术博物馆

![](img/216890131d46798dfe1cd5f886cc608c_200_0.png)

1 of 1

Group
Museum

Address
1000 Fifth Avenue, New York, NY, USA

URL
https://www.metmuseum.org

40.77916, -73.96292

添加图像或视频

15. 你可以将底图样式更改为 Google 提供的九种不同版本之一。为底图背景和标记图标选择高对比度颜色。

## 底图

更改底图的样式

16. 在地图顶部，可看到手动添加更多点标记、绘制线条、添加路线或测量距离的按钮。然而，Google My Maps 对折线和多边形的支持有限，且无法轻松创建用代表数据值的彩色边界表示的等值分区地图。

绘制线条

17. 点击 Preview 查看地图对其他人的显示效果。当你完成地图编辑后，点击地图标题和描述下方的 Share，并在下一屏幕中确保 “Enable link sharing” 已激活，并复制生成的链接。你可以与任何人分享该链接，无论是否拥有 Google 账户。你还有选择让地图在网络搜索结果中公开显示。

![](img/216890131d46798dfe1cd5f886cc608c_202_0.png)

18. 如果你希望将地图作为 iframe 嵌入到网页中，点击地图标题右侧的三按钮 kebab 菜单并选择 “Embed on my site”。这将生成 HTML 嵌入代码，我们将在第 9 章中解释。

19. 如果你希望在未来编辑地图，登录你的 Google 账户后有两种访问方式。一种是打开 Google My Maps 平台以查看所有地图。另一种是前往你的 Google Drive 并按关键字搜索你的 Google My Maps。当你从 Google Sheet 中的数据创建 Google My Map 时，我们建议将 My Map 和 Sheet 文件保存在 Google Drive 的同一文件夹中，以便将来更轻松地进行编辑。

![](img/216890131d46798dfe1cd5f886cc608c_202_1.png)

Google My Maps 是学习制作交互式地图，尤其是带自定义图标的点地图的良好入门工具。你可以设计包含多层点、折线和基本多边形的地图（如果需要）。但整体地图设计和功能仅限于 Google My Maps 平台提供的功能。了解更多信息，请访问 Google My Maps 支持页面。

在下一节中，我们将探讨如何使用 Datawrapper 创建符号点地图，其中每个圆圈（或其他形状）的大小和颜色代表该特定点的数值。

## 使用 Datawrapper 的符号点地图

