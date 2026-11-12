
```json
], [ -74.049236298420453, 40.565362736368101 ], [
-74.050026201586434, 40.565318180621134 ], [ -74.050906017050892,
40.566094342130597 ], [ -74.050679167486138, 40.566310845736403
], [ -74.05107159803778, 40.566722493397798 ], [
-74.050508064032471, 40.566422034160816 ] ] ] } }
',
{ "type": "Feature", "id": 1, "properties": { "boroughCode": 5,
"borough": "Staten Island", "@id":
"http://\\nyc.pediacities.com\\Resource\\Borough\\Staten_Island"
}, "geometry": { "type": "Polygon", "coordinates": [ [ [
-74.053140368211089, 40.57702715545755 ], [ -74.054060449398747,
40.5771164445238872 ], [ -74.054897782108043, 40.577824440919812
], [ -74.054693169074866, 40.579691632229434 ], [
-74.054851346253912, 40.579707593077607 ], [ -74.054845600521745,
40.579945791427605 ], [ -74.053686291504192, 40.580548759614409
], [ -74.052931906398172, 40.57990247466585 ], [
-74.053140368211089, 40.57702715545755 ] ] ] } }
```

## 地理空间数据格式

存在不同的专用地理空间格式。此外，诸如带地理标记的日志和文本数据等数据源也可用于收集位置数据。这些包括：

- 矢量格式，如 GeoJSON、KML、Shapefile 和 WKT
- 栅格格式，如 ESRI Grid、GeoTIFF、JPEG 2000 和 NITF
- 导航标准，如 AIS 和 GPS 设备所使用的标准
- 可通过 JDBC / ODBC 连接访问的地理数据库，如 PostgreSQL / PostGIS
- 来自高光谱、多光谱、激光雷达和雷达平台的遥感传感器格式
- OGC Web 标准，如 WCS、WFS、WMS 和 WMTS
- 带地理标记的日志、图片、视频和社交媒体
- 带有位置引用的非结构化数据

## Geopandas

选择用于执行地理空间分析的库时，首先应考虑您需要处理哪种类型的数据。我们需要一个能够解析 GeoJSON 数据并处理空间关系的库，例如检测给定的经度/纬度对是否位于表示特定行政区边界的多边形内。我们将使用 Geopandas 库来完成此任务。GeoPandas 是一个开源项目，旨在使在 Python 中处理地理空间数据更加容易。它扩展了 pandas 库使用的数据类型（我们在前面的章节中使用过该库），以便对几何数据类型进行空间操作。

使用 pip 安装 GeoPandas 包。

```
pip3 install geopandas
```

让我们现在开始检查出租车数据的地理空间方面。对于每次行程，我们都有表示乘客上车和下车地点的经度/纬度对。我们希望能够确定这些经度/纬度对中的每一个属于哪个行政区，并识别出没有在五个行政区中任何一个开始或结束的行程。例如，如果一辆出租车从曼哈顿载客前往纽瓦克国际机场，那将是一次有效的行程，值得分析，尽管它不会在五个行政区中的任何一个结束。

要执行我们的行政区分析，我们需要加载之前下载并存储在 *nyc-boroughs.geojson* 文件中的 GeoJSON 数据。

```
import geopandas as gdp
gdf = gdp.read_file("./data/trip_data_ch07/nyc-boroughs.geojson")
```

在将 GeoJSON 特性应用于出租车行程数据之前，我们应花点时间思考如何以最高效率组织这些地理空间数据。一个选择是研究针对地理空间查找优化的数据结构，例如四叉树，然后查找或编写自己的实现。相反，我们将尝试提出一个快速的启发式方法，以绕过这部分工作。

我们将遍历 `gdf`，直到找到某个特征，其几何图形包含给定的经度/纬度点。纽约市的大多数出租车行程都开始和结束于曼哈顿，因此如果表示曼哈顿的地理空间特性出现在序列的较早位置，我们的搜索将相对较快地结束。我们可以利用每个特性的 `boroughCode` 属性作为排序键，其中曼哈顿的代码为 1，史泰登岛的代码为 5。在每个行政区的特性范围内，我们希望与最大多边形关联的特性排在较小的多边形前面，因为大多数行程都会前往或来自每个行政区的“主要”区域。

```python
gdf = gdf.to_crs(3857)
gdf['area'] = gdf.apply(lambda x: x['geometry'].area, axis=1)
gdf.head(5)
```

| | boroughCode | borough | @id | geometry | area |
|---|---|---|---|---|---|
| 0 | 5 | 史泰登岛 | http://nyc.pediacities.com/Resource/Borough/St... | POLYGON ((-8243264.851 4948597.836, -8243206.4... | 1.997039e+04 |
| 1 | 5 | 史泰登岛 | http://nyc.pediacities.com/Resource/Borough/St... | POLYGON ((-8243557.877 4950251.047, -8243660.3... | 8.180157e+04 |
| 2 | 5 | 史泰登岛 | http://nyc.pediacities.com/Resource/Borough/St... | POLYGON ((-8255392.882 4959598.361, -8255451.0... | 1.248422e+05 |
| 3 | 5 | 史泰登岛 | http://nyc.pediacities.com/Resource/Borough/St... | POLYGON ((-8246794.198 4960600.673, -8246706.2... | 2.616393e+08 |
| 4 | 4 | 皇后区 | http://nyc.pediacities.com/Resource/Borough/Qu... | POLYGON ((-8219461.925 4952778.732, -8219464.8... | 6.478373e+05 |

按 borough 代码和每个特征几何图形面积的组合对特性进行排序应该就能奏效：

```python
gdf = gdf.sort_values(by=['boroughCode', 'area'], ascending=[True, False])
gdf.head(5)
```

| | boroughCode | borough | @id | geometry | area |
|---|---|---|---|---|---|
| 72 | 1 | 曼哈顿 | http://nyc.pediacities.com/Resource/Borough/Ma... | POLYGON ((-8229449.824 4994307.583, -8229437.5... | 9.588180e+07 |
| 71 | 1 | 曼哈顿 | http://nyc.pediacities.com/Resource/Borough/Ma... | POLYGON ((-8229449.824 4994307.583, -8229437.5... ((-8228885.652 4983011.624, -8228771.7... | 3.809060e+06 |
| 51 | 1 | 曼哈顿 | http://nyc.pediacities.com/Resource/Borough/Ma... | POLYGON ((-8239506.649 4967214.563, -8239357.0... | 1.242664e+06 |
| 69 | 1 | 曼哈顿 | http://nyc.pediacities.com/Resource/Borough/Ma... | POLYGON ((-8231163.561 4978335.590, -8231280.9... | 1.019522e+06 |
| 73 | 1 | 曼哈顿 | http://nyc.pediacities.com/Resource/Borough/Ma... | POLYGON ((-8227250.755 4994032.424, -8227372.2... | 5.352348e+05 |

请注意，我们根据面积值按降序排序，因为我们希望最大的多边形排在前面，而 sort_values 默认按升序排序。

现在我们可以将 gdf GeoPandas 数据帧中排序后的特性广播到集群，并编写一个函数，使用这些特性来找出特定行程结束于哪个行政区（如果有的话）：

```
b_gdf = spark.sparkContext.broadcast(gdf)

def find_borough(latitude,longitude):
    mgdf = b_gdf.value.apply(lambda x: x['borough'] if
x['geometry'].intersects(gdp.points_from_xy([longitude],
[latitude])[0]) else None, axis=1)
    idx = mgdf.first_valid_index()
    return mgdf.loc[idx] if idx is not None else None

find_borough_udf = fun.udf(find_borough, StringType())
```

## 使用 UDF 还是不使用 UDF？

PySpark SQL 使将业务逻辑内联为可从标准 SQL 使用的函数变得非常简单，就像我们在此处使用 `find_borough` 函数一样。鉴于此，您可能认为将所有业务逻辑都移到 UDF 中是个好主意，以便于重用、测试和维护。但是，在开始在代码中到处使用 UDF 之前，有几点关于使用 UDF 的注意事项需要留意。

- 首先，UDF 对 PySpark 的 SQL 查询规划器和执行引擎是不透明的，而标准 SQL 查询语法则不是，因此将逻辑移入 UDF 而不是使用字面 SQL 表达式可能会损害查询性能。
- 其次，在 PySpark SQL 中处理空值可能会迅速变得复杂，尤其是对于接受多个参数的 UDF。

我们可以将 `find_borough` 应用于 `taxi_raw` DataFrame 中的行程，以创建按行政区划分的行程直方图：

```
df_with_boroughs = taxi_raw.withColumn("dropoff_borough",
find_borough_udf(fun.col("dropoff_latitude"),\
fun.col('dropoff_longitude')))

df_with_boroughs.groupBy(fun.col("dropoff_borough")).count().show()
()
...
```

| dropoff_borough | count |
|---|---|
| 皇后区 | 672192 |
| null | 7942421 |
| 布鲁克林 | 715252 |
| 史泰登岛 | 3338 |
| 曼哈顿 | 12979047 |
| 布朗克斯 | 67434 |
