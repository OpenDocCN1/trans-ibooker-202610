

我们打印前 10 个震级值，这样就能确认是否获取到了正确的数据：

```python
[1.6, 1.6, 2.2, 3.7, 2.92000008, 1.4, 4.6, 4.5, 1.9, 1.8]
```

接下来，我们将提取每次地震的位置数据，然后绘制地震分布图。

## 提取位置数据

每次地震的位置数据都存储在键`"geometry"`下。几何字典中包含一个`"coordinates"`键，该列表中的前两个值分别是经度和纬度。我们将按以下方式提取这些数据：

```python
# eq_explore_data.py
--snip--
all_eq_dicts = all_eq_data['features']

mags, lons, lats = [], [], []
for eq_dict in all_eq_dicts:
    mag = eq_dict['properties']['mag']
    lon = eq_dict['geometry']['coordinates'][0]
    lat = eq_dict['geometry']['coordinates'][1]
    mags.append(mag)
    lons.append(lon)
    lats.append(lat)

print(mags[:10])
print(lons[:5])
print(lats[:5])
```

我们创建了用于存储经度和纬度的空列表。代码`eq_dict['geometry']`访问表示地震几何元素的字典❶。第二个键`'coordinates'`提取与`'coordinates'`关联的值列表。最后，索引`0`请求坐标列表中的第一个值，该值对应于地震的经度。

打印前 5 个经度和纬度时，输出结果表明我们获取到了正确的数据：

```python
[1.6, 1.6, 2.2, 3.7, 2.92000008, 1.4, 4.6, 4.5, 1.9, 1.8]
[-150.7585, -153.4716, -148.7531, -159.6267, -155.248336791992]
[61.7591, 59.3152, 63.1633, 54.5612, 18.7551670074463]
```

有了这些数据，我们就可以继续绘制每次地震的位置。

## 构建世界地图

利用目前提取出的信息，我们可以构建一张简单的世界地图。虽然它现在还不够美观，但在关注样式和呈现方式之前，我们需要先确保信息显示正确。下面是初始地图：

```python
# eq_world_map.py
from pathlib import Path
import json

import plotly.express as px

--snip--
for eq_dict in all_eq_dicts:
    --snip--

title = 'Global Earthquakes'
fig = px.scatter_geo(lat=lats, lon=lons, title=title)
fig.show()
```

我们使用别名`px`导入`plotly.express`，这与第 15 章中的做法相同。`scatter_geo()`函数❶允许你将地理数据的散点图叠加到地图上。在最简单的使用方式中，你只需要提供一个纬度列表和一个经度列表。我们将列表`lats`传递给`lat`参数，将`lons`传递给`lon`参数。

运行此文件后，你应该会看到一张类似图 16-7 的地图。这再次展现了 Plotly Express 库的强大之处：只需三行代码，我们就创建了一张全球地震活动地图。

![](img/8f15563c16d2a98c4d3115150679b092_564_0.png)

图 16-7：显示过去 24 小时内所有地震发生位置的简单地图

现在，我们已经确认数据集中的信息被正确绘制出来了，接下来可以进行一些修改，让地图更有意义、更易于阅读。

## 表示震级

地震活动地图应该显示每次地震的震级。既然我们已经知道数据能够被正确绘制，现在还可以加入更多数据。

![](img/8f15563c16d2a98c4d3115150679b092_566_0.png)

图 16-8：现在，地图显示了过去 30 天内所有地震的震级。

这张地图有所改进，但仍然很难分辨哪些点代表震级较大的地震。我们还可以使用颜色来表示震级，从而进一步改进地图。

## 自定义标记颜色

我们可以使用 Plotly 的颜色比例，根据相应地震的严重程度自定义每个标记的颜色。同时，我们还会为底图使用不同的投影方式。

```python
# eq_world_map.py
--snip--
fig = px.scatter_geo(
    lat=lats, lon=lons, size=mags, title=title,
    color=mags,
    color_continuous_scale='Viridis',
    labels={'color':'Magnitude'},
    projection='natural earth',
)
fig.show()
```

这里所有重要的变化都发生在`px.scatter_geo()`函数调用中。`color`参数告诉 Plotly 应该使用哪些值来确定每个标记在颜色比例中的位置❶。我们使用`mags`列表来确定每个点的颜色，这与使用`size`参数时的做法相同。

`color_continuous_scale`参数告诉 Plotly 使用哪种颜色比例❷。*Viridis*是一种从深蓝色渐变到亮黄色的颜色比例，非常适合这个数据集。默认情况下，地图右侧的颜色比例标记为*color*；这无法准确表示这些颜色的实际含义。第 15 章介绍过的`labels`参数接受一个字典作为值❸。在这张图中，我们只需要设置一个自定义标签，确保颜色比例显示为*Magnitude*而不是*color*。

我们再添加一个参数，用于修改绘制地震位置的底图。`projection`参数接受多种常见的地图投影❹。这里我们使用`'natural earth'`投影，它会使地图两端呈圆弧形。此外，请注意最后一个参数后面的逗号。当函数调用包含跨越多行的长参数列表时，通常会添加一个末尾逗号，这样就可以随时在下一行添加其他参数。

现在运行程序，你会看到一张美观得多的地图。在图 16-9 中，颜色比例表示每次地震的严重程度；最严重的地震以浅黄色点显示，与许多深色点形成对比。你还可以看出世界上哪些地区的地震活动更加显著。

![](img/8f15563c16d2a98c4d3115150679b092_568_0.png)

图 16-9：在 30 天的地震数据中，使用颜色和大小表示每次地震的震级。

### 其他颜色比例

你还可以选择许多其他颜色比例。要查看可用的颜色比例，请在 Python 终端会话中输入以下两行：

```python
>>> import plotly.express as px
>>> px.colors.named_colorscales()
['aggrnyl', 'agsunset', 'blackbody', ..., 'mygbm']
```

你可以在地震地图中尝试这些颜色比例，也可以将它们用于任何能够通过连续变化的颜色来展示数据模式的数据集。

## 添加悬停文本

为了完成这张地图，我们将添加一些信息性文本，当你将鼠标悬停在表示某次地震的标记上时，这些文本就会显示出来。除了默认显示的经度和纬度外，我们还会显示震级，并提供近似位置的描述。

要进行这项修改，我们需要从文件中提取更多数据：

```python
# eq_world_map.py
--snip--
mags, lons, lats, eq_titles = [], [], [], []
for eq_dict in all_eq_dicts:
    mag = eq_dict['properties']['mag']
    lon = eq_dict['geometry']['coordinates'][0]
    lat = eq_dict['geometry']['coordinates'][1]
    eq_title = eq_dict['properties']['title']
    mags.append(mag)
    lons.append(lon)
    lats.append(lat)
    eq_titles.append(eq_title)

# --snip--
title = 'Global Earthquakes'
fig = px.scatter_geo(lat=lats, lon=lons, size=mags, title=title,
        projection='natural earth',
        hover_name=eq_titles,
)
fig.show()
```

首先，我们创建一个名为`eq_titles`的列表，用于存储每次地震的标题❶。数据中的`'title'`部分包含每次地震的震级和位置的描述性名称，以及其经度和纬度。我们提取这些信息，并将其赋给变量`eq_title`❷，然后将其追加到列表`eq_titles`中。

在`px.scatter_geo()`调用中，我们将`eq_titles`传递给`hover_name`参数❸。现在，Plotly 会将每次地震标题中的信息添加到每个点的悬停文本中。运行此程序后，将鼠标悬停在任意标记上，你应该能够看到该地震发生位置的描述，并读取其确切震级。图 16-10 展示了这种信息的一个示例。

![](img/8f15563c16d2a98c4d3115150679b092_570_0.png)

图 16-10：*悬停文本现在包含了每次地震的摘要。*

这非常令人印象深刻！不到 30 行代码，我们就创建了一张具有视觉吸引力且信息丰富的全球地震活动地图，同时还展示了地球的地质结构。Plotly 提供了多种自定义可视化外观和行为的方式。利用 Plotly 提供的众多选项，你可以制作出完全符合需求的图表和地图。

