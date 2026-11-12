

![](img/c523d33c8cf752e889378f812513252c_98_0.png)

*图 4-3. 机器人的下一步决策树*

现在，九个示例中有七个是正确的。当然，可以不断添加决策规则，直到所有九个示例都被正确预测。生成的决策树所体现的逻辑，如果转换成日常用语，听起来可能会令人难以置信：“如果动物的重量小于 100kg，且颜色是棕色而非绿色，并且腿少于 10 条，那么是的，它是一只合适的宠物。” 虽然能完美拟合给定示例，但这样的决策树会无法预测一只棕色、四条腿的小型鼬獾并非合适宠物。需要某种平衡来避免这种现象，即过拟合。

决策树推广为一种更强大的算法，称为随机森林。随机森林结合许多决策树，以降低过拟合的风险，并分别训练这些决策树。该算法将随机性注入训练过程，使每棵决策树略有不同。结合预测结果可降低预测的方差，使生成的模型更具泛化能力，并提升测试数据上的性能。

以上关于决策树和随机森林的介绍已经足够，我们可以开始在 PySpark 中使用它们了。在下一节中，我们将介绍将要使用的数据集，并为其在 PySpark 中的使用做好准备。

## 准备数据

本章使用的数据集是著名的 Covtype 数据集，可在线获取，为压缩的 CSV 格式数据文件 *covtype.data.gz*，以及伴随的信息文件 *covtype.info*。

该数据集记录了美国科罗拉多州森林覆盖土地块的类型。数据集涉及真实世界森林仅仅是个巧合！每条数据记录包含描述每个土地块的若干特征——例如其海拔、坡度、距水体距离、遮荫情况和土壤类型——以及覆盖该土地的已知森林类型。森林覆盖类型需要从其余特征中预测得出，这些特征总共有 54 个。

该数据集已用于研究，甚至用于 Kaggle 竞赛。在这个章节中探索它是一个有趣的数据集，因为它同时包含类别特征和数值特征。数据集中包含 581,012 个示例，虽然不完全符合大数据的定义，但作为示例已经足够大，便于管理，同时仍能凸显一些规模问题。

幸运的是，数据已经是简单的 CSV 格式，不需要太多清洗或其他准备即可与 PySpark MLlib 一起使用。*covtype.data* 文件应被解压并复制到您的本地或云存储中（例如 AWS S3）。

启动 `pyspark-shell`。您可能会发现，为 shell 分配充足的内存非常有帮助，因为构建决策森林可能资源密集。如果内存充足，请指定 `--driver-memory 8g` 或类似选项。

CSV 文件本质上包含表格数据，按列组织成行。有时这些列会在标题行中被赋予名称，但本例并非如此。列名称在配套文件 covtype.info 中给出。从概念上讲，CSV 文件的每一列也有一种类型——数字、字符串——但 CSV 文件并未指定此信息。

将此数据解析为数据帧很自然，因为这是 PySpark 对表格数据的抽象，带有定义好的列架构，包括列名和类型。PySpark 内置了读取 CSV 数据的支持。让我们使用内置的 CSV 读取器将数据集读取为 DataFrame：

```
data_without_header = spark.read.option("inferSchema",
True).option("header", False).csv("data/covtype.data")
data_without_header.printSchema()
...
root
|-- _c0: integer (nullable = true)
|-- _c1: integer (nullable = true)
|-- _c2: integer (nullable = true)
|-- _c3: integer (nullable = true)
|-- _c4: integer (nullable = true)
|-- _c5: integer (nullable = true)
...
```

此代码将输入读取为 CSV，并不尝试将第一行解析为列名标题。它还要求通过检查数据来推断每一列的类型。它正确推断出所有列都是数字，更具体地说，是整数。不幸的是，它只能将列命名为“_c0”等等。

我们可以查看 covtype.info 文件以获取列名。

```
$ cat data/covtype.info
```

...
[...]
7. 属性信息：
给出了属性名称、属性类型、度量单位
以及
简要描述。森林覆盖类型是分类问题。此列表的顺序与数据库行中数字的顺序对应。

| 名称 | 描述 | 数据类型 | 度量单位 |
| :--- | :--- | :--- | :--- |
| Elevation | 米为单位的海拔 | 定量 | 米 |
| Aspect | 方位角（度） | 定量 | 方位角 |
| Slope | 坡度（度） | 定量 | 度 |
| Horizontal_Distance_To_Hydrology | 到最近地表水体特征的水平距离 | 定量 | 米 |
| Vertical_Distance_To_Hydrology | 到最近地表水体特征的垂直距离 | 定量 | 米 |
| Horizontal_Distance_To_Roadways | 到最近公路的水平距离 | 定量 | 米 |
| Hillshade_9am | 夏至日上午 9 点的山体阴影索引 | 定量 | 0 到 255 |
| Hillshade_Noon | 夏至日正午的山体阴影索引 | 定量 | 0 到 255 |
| Hillshade_3pm | 夏至日下午 3 点的山体阴影索引 | 定量 | 0 到 255 |
| Horizontal_Distance_To_Fire_Points | 到最近野火点火点的水平距离 | 定量 | 米 |
| Wilderness_Area (4 binary columns) | 荒野地区指定（存在为 1） | 定性 | 0（缺失） |
| Soil_Type (40 binary columns) | 土壤类型指定（存在为 1） | 定性 | 0（缺失） |
| Cover_Type (7 types) | 森林覆盖类型指定 | 整数 | 1 到 7 |

查看列信息后，很明显有些特征确实是数值型的。“Elevation”是以米为单位的海拔；“Slope”以度为单位进行测量。但是，“Wilderness_Area”有所不同，因为据说它跨越四列，每列的值为 0 或 1。实际上，“Wilderness_Area”是一个类别值，而非数值。

这四列实际上是独热编码或 1-of-n 编码。当这种编码形式应用于类别特征时，一个取 N 个不同值的类别特征将变为 N 个数值特征，每个特征的值为 0 或 1。N 个值中恰好有一个值为 1，其余均为 0。例如，一个可以是多云、雨天或晴天的天气类别特征将变为三个数值特征，其中多云表示为 1, 0, 0；雨天表示为 0, 1, 0；以此类推。这三个数值特征可以被视为 `is_cloudy`、`is_rainy` 和 `is_clear` 特征。同样，40 列实际上只是一个 `Soil_Type` 类别特征。

这不是将类别特征编码为数字的唯一可能方式。另一种可能的编码只是为类别特征的每个可能值分配一个不同的数值。例如，`cloudy` 可能变为 1.0，`rainy` 变为 2.0，以此类推。目标本身，“Cover_Type”，是编码为 1 到 7 值的类别值。

> 在将类别特征编码为单个数值特征时要小心。原始类别值没有顺序，但编码为数字后，它们似乎有了顺序。将编码后的特征视为数值会导致无意义的结果，因为算法实际上假装 `rainy` 以某种方式大于 `cloudy` 并且是其两倍大。只要编码的数值不作为数字使用，这样做就没问题。

我们已经见过这两种类别特征编码类型。也许，直接不编码此类特征（而且是两种方式），而仅仅像“Rawah 荒野地区”这样直接包含它们的值，会更简单、更直接。这可能是历史遗留产物；该数据集发布于 1998 年。出于性能原因或为了匹配当时库所期望的格式（这些库更多是为回归问题构建的），数据集通常包含以这种方式编码的数据。

无论如何，在继续之前，为该 DataFrame 添加列名很有用，以便更轻松地使用它：

```
from pyspark.sql.types import DoubleType
from pyspark.sql.functions import col

colnames = ["Elevation", "Aspect", "Slope",
"Horizontal_Distance_To_Hydrology",
"Vertical_Distance_To_Hydrology", "Horizontal_Distance_To_Roadways",
"Hillshade_9am", "Hillshade_Noon", "Hillshade_3pm",
"Horizontal_Distance_To_Fire_Points"] + \
[f"Wilderness_Area_{i}" for i in range(4)] + \
[f"Soil_Type_{i}" for i in range(40)] + \
["Cover_Type"]

data = data_without_header.toDF(*colnames).withColumn("Cover_Type",
col("Cover_Type").cast(DoubleType()))
```

