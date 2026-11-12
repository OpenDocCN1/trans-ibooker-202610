# 第 7 章。出租车行程数据的地理空间和时间数据分析

## 早期发行版读者须知

通过早期发行版电子书，你可以获得书籍最早期的形式——作者在编写过程中的原始未编辑内容——因此你可以在这些书籍正式发布很久之前就利用这些技术。

这将是最终图书的第 7 章。

地理空间数据是指以某种形式嵌入位置信息的数据。此类数据目前由数十亿个来源（如手机和传感器）每天大规模生成。关于人类和机器移动以及遥感的数据，对我们的经济和整体福祉至关重要。地理空间分析可以为我们提供理解所有这些数据并将其用于解决各种规模所面临问题所需的工具和方法。

PySpark 和 PyData 生态系统在过去几年中在地理空间分析方面取得了长足进步。它们正在各个行业中用于处理富含位置的数据，进而影响我们的日常生活。地理空间数据以可见方式体现的一个日常活动是本地交通。近年来数字叫车服务的普及现象让我们更加意识到地理空间技术。在本章中，我们将在此领域使用 PySpark 和数据分析技能。我们将使用一个包含纽约市出租车行程信息的数据集。





理解出租车经济学的一个重要统计指标是 *利用率*：出租车上路且载有一名或多名乘客的时间比例。影响利用率的一个因素是乘客的目的地：中午在联合广场附近送乘客的出租车，一两分钟内就能找到下一单生意的可能性要大得多；而凌晨 2 点在斯坦顿岛（Staten Island）送下乘客的出租车，可能得开回曼哈顿才能找到下一单。我们希望量化这些影响，找出出租车在将乘客送至哪个行政区——曼哈顿（Manhattan）、布鲁克林（Brooklyn）、皇后区（Queens）、布朗克斯（the Bronx）、斯坦顿岛（Staten Island），或上述都不是（例如，将乘客送到城市外的地方，如纽瓦克国际机场，Newark International Airport）之后，找到下一单所需的平均时间。

我们将先构建数据集，然后深入进行地理空间分析。我们将学习 GeoJSON 格式，并结合 PySpark 使用 PyData 生态系统中的工具。我们将使用 GeoPandas 处理 *地理空间信息*，例如经纬度点和空间边界。最后，我们将处理数据的时间特征，如日期和时间，通过一种称为会话化（sessionization）的分析方式。这将帮助我们理解纽约市出租车的利用率。PySpark 的 DataFrame API 提供了开箱即用的数据类型和方法来处理时间数据。

让我们开始下载数据集，并使用 PySpark 探索它。

## 纽约市出租车行程数据集的由来

纽约市居民根据自身经验，总结出各种关于什么时间、什么地点更容易打到出租车的小窍门，尤其是在高峰时段和下雨天。但一天中有一个时段，所有人都会建议你干脆坐地铁：那就是每天下午 4 点到 5 点之间的换班时间。在此期间，黄色出租车必须返回调度中心（通常在皇后区），以便一名司机下班、另一名司机开始工作，而返回迟到的司机需要支付罚款。

2014 年 3 月，纽约市出租车和豪轿委员会在其 Twitter 账号 @nyctaxi 上分享了一张信息图，显示了路上出租车的数量以及任何给定时间被占用的出租车比例。果然，从下午 4 点到 6 点，路上出租车数量出现明显下降，且行驶中的出租车有三分之二处于载客状态。

这条推文引起了自诩为城市主义者、制图师和数据狂热者的 Chris Whong 的注意，他向 @nyctaxi 账号发推询问，用于制作该信息图的数据是否公开可用。Chris 获得了 2013 年 1 月 1 日至 12 月 31 日所有出租车行程数据的访问权限。更棒的是，他将所有车费数据发布到网上，这些数据已成为许多纽约市交通可视化作品的基础。这就是本章中我们将使用的数据集。

### 准备数据

本分析中，我们只考虑 2013 年 1 月的车费数据，解压后大约为 2.5 GB。你可以访问 2013 年每个月的数据，如果你手头有足够大的 PySpark 集群，也可以对全年所有数据重现以下分析。现在，让我们在客户端机器上创建一个工作目录，并查看车费数据的结构：

```
$ mkdir taxidata
$ cd taxidata
$ curl -O https://storage.googleapis.com/aas-data-sets/trip_data_1.csv.zip
$ unzip trip_data_1.csv.zip
$ head -n 5 trip_data_1.csv
...
medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
89D227B655E5C82AAECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
...
```

文件中除表头外的每一行都以 CSV 格式表示一次出租车行程。对于每次行程，我们都有出租车的一些属性（车牌号的哈希版本）以及司机的一些属性（执照号的哈希版本，执照即驾驶出租车所需的许可证），一些关于行程开始和结束时间的时间信息，以及乘客上下车地点的经纬度坐标。

让我们创建 taxidata 目录并将行程数据复制到存储中：

```
$ mkdir taxidata
$ mv trip_data_1.csv taxidata/
```

上面我们使用的是本地文件系统，但对你而言可能不是这样。如今更有可能使用云原生文件系统，例如 AWS S3 或 Google Cloud Storage。在这种情况下，你将分别将数据上传到 S3 或 GCS。

现在启动 PySpark shell：

```
$ pyspark
```

PySpark shell 加载完成后，我们可以从出租车数据创建一个数据集，并查看前几行，就像在其他章节中所做的那样：

```
taxi_raw = pyspark.read.option("header", "true").csv("taxidata")
taxi_raw.show(1, vertical=True)
```

```
...
RECORD 0-----------------------------------------------
medallion | 89D227B655E5C82AE...
hack_license | BA96DE419E711691B...
vendor_id | CMT
rate_code | 1
store_and_fwd_flag | N
pickup_datetime | 2013-01-01 15:11:48
dropoff_datetime | 2013-01-01 15:18:10
passenger_count | 4
trip_time_in_secs | 382
trip_distance | 1.0
pickup_longitude | -73.978165
pickup_latitude | 40.757977
dropoff_longitude | -73.989838
dropoff_latitude | 40.751171
only showing top 1 row
...
```

乍一看，这是一个格式良好的数据集。让我们查看 DataFrame 的架构。

```
taxi_raw.printSchema()
...
root
|-- medallion: string (nullable = true)
|-- hack_license: string (nullable = true)
|-- vendor_id: string (nullable = true)
|-- rate_code: integer (nullable = true)
|-- store_and_fwd_flag: string (nullable = true)
|-- pickup_datetime: string (nullable = true)
|-- dropoff_datetime: string (nullable = true)
|-- passenger_count: integer (nullable = true)
|-- trip_time_in_secs: integer (nullable = true)
|-- trip_distance: double (nullable = true)
|-- pickup_longitude: double (nullable = true)
|-- pickup_latitude: double (nullable = true)
|-- dropoff_longitude: double (nullable = true)
|-- dropoff_latitude: double (nullable = true)
...
```

我们将 `pickup_datetime` 和 `dropoff_datetime` 字段表示为 `String` 类型，并将上下车地点的各自（x，y）坐标存储为各自字段中的 `Double` 类型。我们希望将 datetime 字段作为时间戳，因为这样可以方便地对其进行操作和分析。

### 将 datetime 字符串转换为时间戳

如前所述，PySpark 提供了开箱即用的方法来处理时间数据。具体来说，我们将使用 `to_timestamp` 函数来解析 datetime 字符串并将其转换为时间戳。

```
from pyspark.sql import functions as fun

taxi_raw = taxi_raw.withColumn('pickup_datetime',
fun.to_timestamp(fun.col('pickup_datetime'), "yyyy-MM-dd HH:mm:ss"))
taxi_raw = taxi_raw.withColumn('dropoff_datetime',
fun.to_timestamp(fun.col('dropoff_datetime'), "yyyy-MM-dd HH:mm:ss"))
```

让我们再次查看架构。

```
taxi_raw.printSchema()
...
root
```


| 字段 | 类型 | 可空 |
|---|---|---|
| medallion | string | true |
| hack_license | string | true |
| vendor_id | string | true |
| rate_code | integer | true |
| store_and_fwd_flag | string | true |
| pickup_datetime | timestamp | true |
| dropoff_datetime | timestamp | true |
| passenger_count | integer | true |
| trip_time_in_secs | integer | true |
| trip_distance | double | true |
| pickup_longitude | double | true |
| pickup_latitude | double | true |
| dropoff_longitude | double | true |
| dropoff_latitude | double | true |

pickup_datetime 和 dropoff_datetime 字段现在是时间戳了。干得好！

我们之前提到过，此数据集包含 2013 年 1 月的行程。不过，请不要只凭我们的一面之词。我们可以通过对 pickup_datetime 字段进行排序来获取数据中的最新时间，以此来确认这一点。为此，我们使用 DataFrame 的 sort 方法结合 PySpark 列的 desc 方法。

```
taxi_raw.sort(fun.col("pickup_datetime").desc()).show(3, vertical=True)
```

**记录 0**

| 字段 | 值 |
|---|---|
| medallion | EA00A64CBDB68C77D... |
| hack_license | 2045C77002FA0F2E0... |
| vendor_id | CMT |
| rate_code | 1 |
| store_and_fwd_flag | N |
| pickup_datetime | 2013-01-31 23:59:59 |
| dropoff_datetime | 2013-02-01 00:08:39 |
| passenger_count | 1 |
| trip_time_in_secs | 520 |
| trip_distance | 1.5 |
| pickup_longitude | -73.970528 |
| pickup_latitude | 40.75502 |
| dropoff_longitude | -73.981201 |
| dropoff_latitude | 40.769104 |

**记录 1**

| 字段 | 值 |
|---|---|
| medallion | E3F00BB3F4E710383... |
| hack_license | 10A2B96DE39865918... |
| vendor_id | CMT |
| rate_code | 1 |
| store_and_fwd_flag | N |
| pickup_datetime | 2013-01-31 23:59:59 |
| dropoff_datetime | 2013-02-01 00:05:16 |
| passenger_count | 1 |
| trip_time_in_secs | 317 |
| trip_distance | 1.0 |
| pickup_longitude | -73.990685 |
| pickup_latitude | 40.719158 |
| dropoff_longitude | -74.003288 |
| dropoff_latitude | 40.71521 |

**记录 2**

| 字段 | 值 |
|---|---|
| medallion | 83D8E776A05EEF731... |
| hack_license | E6D27C8729EF55D20... |
| vendor_id | CMT |
| rate_code | 1 |
| store_and_fwd_flag | N |
| pickup_datetime | 2013-01-31 23:59:58 |
| dropoff_datetime | 2013-02-01 00:04:19 |
| passenger_count | 1 |
| trip_time_in_secs | 260 |
| trip_distance | 0.8 |
| pickup_longitude | -73.982452 |
| pickup_latitude | 40.772777 |
| dropoff_longitude | -73.989227 |
| dropoff_latitude | 40.766754 |

仅显示前 3 行 ...

在设置好数据类型后，让我们检查一下数据中是否存在任何不一致。

## 处理无效记录

任何处理过大规模真实世界数据集的人都知道，这些数据集几乎总会包含至少少数几条不符合编写处理代码的人所期望的记录。许多 PySpark 管道因无效记录而失败，这些无效记录导致解析逻辑抛出异常。在进行交互式分析时，我们可以通过关注关键变量来了解数据中潜在异常的情况。

在我们的情况中，包含地理空间和时间信息的变量值得检查不一致之处。这些列中存在空值肯定会影响我们的分析。

```
geospatial_temporal_colnames = ["pickup_longitude",
"pickup_latitude", "dropoff_longitude", "dropoff_latitude",
"pickup_datetime", "dropoff_datetime"]
taxi_raw.select([fun.count(fun.when(fun.isnull(c), c)).alias(c)
for c in geospatial_temporal_colnames]).show()
...
```

| pickup_longitude | pickup_latitude | dropoff_longitude | dropoff_latitude | pickup_datetime | dropoff_datetime |
|---|---|---|---|---|---|
| 0 | 0 | 86 | 86 | 0 | 0 |

让我们从数据中删除空值。

```
taxi_raw = taxi_raw.na.drop(subset=geospatial_temporal_colnames)
```

我们可以进行的另一项常识检查是针对纬度和经度值为零的记录。我们知道，对于我们关注的地区来说，这些值是无效的。

```
print("Count of zero dropoff, pickup latitude and longitude records")
taxi_raw.groupBy( (fun.col("dropoff_longitude") == 0) |
(fun.col("dropoff_latitude") == 0) | (fun.col("pickup_longitude") == 0) | (fun.col("pickup_latitude") == 0) ).\
count().show()
...
```

| (((dropoff_longitude = 0) OR (dropoff_latitude = 0)) OR (pickup_longitude = 0)) OR (pickup_latitude = 0) | count |
|---|---|
| true | 285909 |
| false | 14490620 |

- 1. 如果任一条件对任意记录求值为 True，则多个 OR 条件将为真

我们有相当多此类记录。如果看起来出租车把乘客带到了南极，我们可以相当有把握地认为该记录无效，并应将其从分析中排除。我们不会删除它们，而是会在下一部分末尾再回来处理，看看它们如何影响我们的分析。

在生产环境中，我们通过逐个检查单个任务的日志来处理这些异常，找出是哪一行代码抛出了异常，然后想办法修改代码以忽略或纠正无效记录。这是一个繁琐的过程，往往感觉像是在玩打地鼠游戏：刚修复一个异常，就会在分区中稍后到达的另一条记录上发现另一个异常。

有经验的数据科学家在处理新数据集时采用的一种策略是在解析代码中添加 `try-except` 块，以便任何无效记录都可以写入日志，而不会导致整个作业失败。如果整个数据集中只有少数几条无效记录，我们或许可以忽略它们并继续分析。

既然我们已经准备好数据集，让我们开始进行地理空间分析。

## 地理空间分析

地理空间数据主要有两种类型——矢量和栅格——针对每种类型都有不同的工具。在我们的情况中，我们有出租车行程记录的纬度和经度，以及以 GeoJSON 格式存储的表示纽约市各自治市边界的矢量数据。我们已经查看了纬度和经度点。让我们先来看看 GeoJSON 数据。

### GeoJSON 简介

我们将用于纽约市各自治市边界的数据采用名为 GeoJSON 的格式编写。GeoJSON 中的核心对象称为 feature，由一个几何实例和一组名为 properties 的键值对组成。几何是点、线或多边形等形状。一组 feature 称为 FeatureCollection。让我们下载纽约市自治市地图的 GeoJSON 数据并查看其结构。

在客户端机器的 taxidata 目录中，下载数据并将文件重命名为稍短的名称：

```
$ curl -O https://nycda...
tastables.s3.amazonaws.com/2013-08-
19T18:15:35.172Z/nyc-borough-boundaries-polygon.geojson
$ mv nyc-borough-boundaries-polygon.geojson nyc-boroughs.geojson
```

打开文件并查看一条要素记录。注意属性和几何对象——在这里，一个多边形表示自治市的边界，属性包含自治市名称和其他相关信息。

```
$ head -n 7 data/trip_data_ch07/nyc-boroughs.geojson
...
{
"type": "FeatureCollection",
"features": [{ "type": "Feature", "id": 0, "properties": {
"boroughCode": 5, "borough": "Staten Island", "@id":
"http://nyc.pediacities.com/Resource/Borough/Staten_Island"
}, "geometry": { "type": "Polygon", "coordinates": [[ [
-74.050508064032471, 40.566422034160816 ], [ -74.049983525625748,
40.566395924928273 ], [ -74.049316403620878, 40.565887747780437
```

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




如我们所料，绝大多数行程在曼哈顿区结束，而在史泰登岛结束的行程相对较少。一个令人惊讶的观察是，结束于任何行政区之外的行程数量；`null` 记录的数量显著多于结束于布朗克斯区的出租车行程数量。

我们之前曾讨论过如何处理此类无效记录，但并未将其删除。我们将删除此类记录并从清理后的数据中创建直方图的任务留给你作为练习。完成后，你会注意到大量 `null` 条目，只剩下相当合理的城市外送客观察数量。

在处理了数据的地理空间方面后，让我们现在通过使用 PySpark 执行会话化，深入探讨数据的时间性质。

## PySpark 中的会话化

这种分析——即分析单个实体随时间执行的一系列事件——称为 *会话化*，通常用于分析网站日志以了解网站用户的行为。PySpark 提供开箱即用的 `Window` 和聚合函数，可用于执行此类分析。这些功能使我们能够专注于业务逻辑，而不是试图实现复杂的数据操作和计算。我们将在下一节中使用这些功能，以更好地理解数据集中出租车的利用情况。

会话化是一种非常强大的技术，用于挖掘数据洞察并构建可帮助人们做出更好决策的新数据产品。例如，Google 的拼写纠正引擎是基于 Google 每天从其网络属性上发生的每个事件（搜索、点击、地图访问等）的记录中构建的用户活动会话构建的。为了识别可能的拼写纠正候选项，Google 处理这些会话，寻找用户输入查询、未点击任何内容、几秒钟后输入略有不同的查询、然后点击结果且未返回 Google 的情况。然后它统计该模式对于任何查询对出现的频率。如果它出现的频率足够高（例如，每次我们看到查询“untied stats”时，几秒钟后都会出现查询“united states”），那么我们假设第二个查询是第一个查询的拼写纠正。

此分析利用事件日志中体现的人类行为模式，从数据构建比任何基于字典创建的引擎都更强大的拼写纠正引擎。该引擎可用于对任何语言进行拼写纠正，并可纠正可能不包含在任何字典中的单词（例如，新创公司的名称），甚至可以纠正“untied stats”这类查询（其中没有任何单词拼写错误）。Google 使用类似的技术来显示推荐和相关搜索，以及决定哪些查询应返回在搜索页面本身上直接给出查询答案的 OneBox 结果，而无需用户点击进入不同页面。天气、体育比赛得分、地址以及许多其他类型的查询都有 OneBox。

到目前为止，关于每个实体发生的一组事件的信息分散在 DataFrame 的各个分区中，因此，为了进行分析，我们需要将这些相关事件放在一起并按时间顺序排列。在下一节中，我们将展示如何使用高级 PySpark 功能高效地构建和分析会话。

## 构建会话：PySpark 中的二次排序

在 PySpark 中创建会话的简单方法是对我们想要创建会话的标识符执行 `groupBy`，然后在洗牌后按时间戳标识符对事件进行排序。如果每个实体只有少量事件，这种方法效果相当不错。但是，由于这种方法要求任何特定实体的所有事件必须同时驻留在内存中，因此随着每个实体的事件数量越来越大，它将无法扩展。我们需要一种构建会话的方法，该方法不要求特定实体的所有事件同时驻留在内存中以进行排序。

在 MapReduce 中，我们可以通过执行 *二次排序* 来构建会话，其中我们创建由标识符和时间戳值组成的组合键，按组合键对所有记录进行排序，然后使用自定义分区器和分组函数，确保相同标识符的所有记录出现在同一个输出分区中。幸运的是，PySpark 也可以通过使用 Window 函数来支持类似的模式。

```
from pyspark.sql import Window
```

```
window_spec =
Window.partitionBy("hack_license").orderBy(fun.col("hack_license"
), fun.col("pickup_datetime"))
```

首先，我们使用 *partitionBy* 方法，确保 *license* 列中具有相同值的所有记录最终出现在同一个分区中。然后，在这些分区中，我们按 *license* 值对记录进行排序（这样同一司机的所有行程会出现在一起），再按 *pickupTime* 排序，以便行程序列在分区内按排序顺序出现。现在，当我们聚合行程记录时，可以确保行程以对会话分析最佳的方式排序。由于此操作会触发洗牌并进行大量计算，并且我们需要多次使用结果，因此我们对其进行缓存：

```
window_spec.cache()
```

执行会话化管道是一项昂贵的操作，而会话化后的数据通常对我们可能想要执行的许多不同分析任务都很有用。在可能希望稍后继续分析或与其他数据科学家协作的场景中，最好通过仅执行一次会话化来分摊对大型数据集进行会话化的成本，然后将会话化后的数据写入 S3 或 HDFS 等文件系统，以便用于回答许多不同的问题。仅执行一次会话化也是在整个数据科学团队中强制执行会话定义标准规则的好方法，这对确保结果可比具有相同的好处。

此时，我们准备分析会话数据，看看司机在某个行政区送客后需要多长时间才能找到下一单乘客。我们将使用前面创建的 `lag` 函数和 `window_spec` 对象，获取两次行程并计算第一次行程的送客时间与第二次行程的接客时间之间的秒数：

```
df_with_borough_durations = df_with_boroughs.\
  withColumn("trip_time_difference", \
  fun.col("pickup_datetime") -
fun.lag(fun.col("pickup_datetime"), 1). \
  over(window_spec) ).show(50, vertical=True)
```

现在，我们应执行验证检查，以确保大多数持续时间均为非负值：

```
df_with_borough_durations.
selectExpr("floor(seconds / 3600) as hours").
groupBy("hours").
count().
sort("hours").
show()
```

| 小时 | 计数 |
|---|---|
| -3 | 2 |
| -2 | 16 |
| -1 | 4253 |
| 0 | 13359033 |
| 1 | 347634 |
| 2 | 76286 |
| 3 | 24812 |
| 4 | 10026 |
| 5 | 4789 |

只有少数记录具有负持续时间，当我们更仔细地检查它们时，似乎没有任何共同模式可以用来了解错误数据的来源。如果我们从输入数据集中排除这些负持续时间记录，并按行政区查看接客时间的平均值和标准差，则会看到：

```
df_with_borough_durations.
where("seconds > 0 AND seconds < 60*60*4").
groupBy("borough").
agg(avg("seconds"), stddev("seconds")).
show()
...
```

| 行政区 | avg(seconds) | stddev_samp(seconds) |
|---|---|---|
| Queens | 2380.660355494727 | 2206.6572799118035 |
| NA | 2006.53571169866 | 1997.0891370324784 |
| Brooklyn | 1365.394576250576 | 1612.9921698951398 |
| Staten Island | 2723.5625 | 2395.7745475546385 |
| Manhattan | 631.8473780726746 | 1042.919915477234 |
| Bronx | 1975.9209786770646 | 1704.006452085683 |

正如我们所料，数据显示曼哈顿的送客对司机来说停机时间最短，约为 10 分钟。结束于布鲁克林的出租车行程停机时间是其两倍多，而相对较少的结束于史泰登岛的行程则需要司机平均近 45 分钟才能接到下一单乘客。

正如数据所显示的，出租车司机有很大的经济动机根据乘客的最终目的地对其进行歧视；特别是史泰登岛的送客会让司机面临大量的停机时间。多年来，纽约市出租车和豪华轿车委员会一直在努力识别这种歧视，并对因乘客想去的地方而拒载乘客的司机处以罚款。尝试检查数据中异常短的出租车行程将很有趣，这些行程可能表明司机和乘客之间关于乘客想去哪里送客存在争议。

## 后续方向

在本章中，我们使用了真实数据集的时间和空间特征。你迄今为止获得的地理空间分析熟悉程度可用于深入研究 GeoSpark 或 GeoMesa 等框架。与使用相比，它们的学习曲线会更陡峭。





GeoPandas 和 UDFs 会更高效。此外，在使用地理空间和时序数据进行数据可视化方面也大有可为。

此外，想象将这项技术应用于出租车数据，构建一个能够根据当前交通模式和数据中包含的历史次优位置记录，推荐出租车在送客后前往最佳地点的应用。你也可以从试图招手叫车的人角度来看待这些信息：鉴于当前时间、地点和天气数据，未来五分钟内我能从街上招到出租车的概率是多少？此类信息可以融入到 Google Maps 等应用中，帮助旅行者决定何时出发以及选择哪种出行方式。
