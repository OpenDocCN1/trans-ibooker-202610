

data.head()
...
Row(Elevation=2596, Aspect=51, Slope=3,
Horizontal_Distance_To_Hydrology=258,
Vertical_Distance_To_Hydrology=0,
Horizontal_Distance_To_Roadways=510, Hillshade_9am=221,
Hillshade_Noon=232, Hillshade_3pm=148, ...)

++ 用于连接集合

与荒野和土壤相关的列命名为“Wilderness_Area_0”、“Soil_Type_0”，一小段 Python 代码即可生成这 44 个名称，无需逐个输入。最后，目标“Cover_Type”列会预先转换为 double 值，因为在所有 PySpark MLlib API 中，实际上需要将其作为 double 而非 int 来消耗。这一点在后面会变得清晰。

你可以调用 data.show() 查看数据集的若干行，但显示过于宽阔，几乎无法阅读。data.head() 将其显示为原始 Row 对象，在这种情况下可读性更高。

现在我们已熟悉数据集并对其进行了处理，可以训练决策树模型了。

## 我们的第一棵决策树

在第 3 章中，我们立即在所有可用数据上构建了一个推荐模型。这创建了一个推荐器，任何具有音乐知识的人都可以对其进行合理性检查：查看用户的收听习惯和推荐，我们就能感觉到它产生了良好的结果。在这里，这不可能。我们完全不知道如何为科罗拉多州的一块新土地编造一个新的 54 特征描述，或者期望从这样一块土地上获得什么样的森林覆盖类型。

相反，我们必须直接留出部分数据用于评估所得模型。之前，AUC 指标用于评估留出的收听数据与推荐预测之间的一致性。AUC 可以被视为随机选取的好推荐排在随机选取的坏推荐之前的概率。这里的原理相同，尽管评估指标不同：准确率。再次，绝大多数——90%——的数据将用于训练，后来我们将看到，这个训练集的一个子集将被留出用于交叉验证（CV 集）。这里留出的另外 10% 实际上是第三个子集，一个正式的测试集。

```
(train_data, test_data) = data.randomSplit([0.9, 0.1])
train_data.cache()
test_data.cache()
```

数据需要做一些额外的准备才能在 MLlib 中与分类器一起使用。输入 DataFrame 包含许多列，每列存储一个可用于预测目标列的特征。MLlib 要求将所有输入收集到一列中，其值为一个向量。这个类是线性代数意义上向量的抽象，仅包含数字。出于大多数目的，它们的工作方式类似于 double 值（浮点数）的简单数组。当然，一些输入特征在概念上是分类的，即使它们在输入中都用数字表示。

幸运的是，VectorAssembler 类可以完成这项工作：

```
from pyspark.ml.feature import VectorAssembler

input_cols = colnames[:-1]
vector_assembler = VectorAssembler(inputCols=input_cols, outputCol="featureVector")

assembled_train_data = vector_assembler.transform(train_data)

assembled_train_data.select("featureVector").show(truncate = False)
...
+------------------------------+
...
| featureVector                 |
...
+------------------------------+
...
| (54, [0, 1, 2, 5, 6, 7, 8, 9, 13, 18], [1874.0, 18.0, 14.0, 90.0, 208.0, 209.0, ...|
| (54, [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 13, 18], [1879.0, 28.0, 19.0, 30.0, 12.0, 95.0, |
...
...
```

### ❶ 排除标签，Cover_Type

其关键参数是要合并为特征向量的列，以及包含特征向量的新列的名称。这里，除目标列外，所有列都作为输入特征包含在内。生成的 DataFrame 有一个新的“featureVector”列，如所示。

输出看起来并不完全像一串数字，但这是因为这里显示的是向量的原始表示形式，它以 `SparseVector` 实例的形式表示以节省存储。由于 54 个值中大部分都是 0，它只存储非零值及其索引。在分类中，这个细节无关紧要。

## 特征向量

考虑根据今天的天气预测明天的最高气温。这个想法本身没有问题，但“今天的天气”是一个随意的概念，在输入到学习算法之前需要进行结构化。实际上，今天天气中的某些 _features_ 可能预测明天的气温，例如最高气温、最低气温、平均湿度、今天是否多云、下雨或晴朗，以及预测明天将出现寒潮的气象预报员人数。

这些特征有时也称为 _dimensions_、_predictors_，或者只是 _variables_。这些特征中的每一个都可以量化。例如，最高和最低气温以摄氏度为单位，湿度可以表示为 0 到 1 之间的分数，天气类型可以标记为 `cloudy`、`rainy` 或 `clear`。预报员人数当然是一个整数计数。因此，今天的天气可以归结为类似 `13.1, 19.0, 0.73, cloudy, 1` 的值列表。

这五个特征按顺序组合在一起，称为 _feature vector_，可以描述任何一天的天气。这种用法在某种程度上与线性代数中 _vector_ 术语的使用相似，只是从这个意义上说，向量可以概念上包含非数值，甚至可能缺少某些值。这些特征并非全都属于同一类型。前两个特征以摄氏度为单位，但第三个是无单位的，是一个分数。第四个根本不是数字，第五个是一个始终为非负整数的数字。

学习算法需要在数据上进行训练才能进行预测。特征向量提供了一种组织化的方式来描述输入给学习算法（例如：`12.5, 15.5, 0.10, clear, 0`）。预测的输出，即 _target_，也可以被视为一个特征。在这里，它是一个数值特征：`17.2`。

将目标仅仅作为特征向量中的另一个特征来包含，这并不罕见。整个训练示例可以被视为 `12.5, 15.5, 0.10, clear, 0, 17.2`。所有这些示例的集合称为 _training set_。

VectorAssembler 是当前 MLlib "Pipelines" API 中 Transformer 的一个示例。它根据某些逻辑将输入 DataFrame 转换为另一个 DataFrame，并可与其他转换组合成一个管道。本章稍后，这些转换将连接成一个实际的 Pipeline。在这里，转换只是被直接调用，这足以构建第一个决策树分类器模型。

```
from pyspark.ml.classification import DecisionTreeClassifier

classifier = DecisionTreeClassifier(seed = 1234,
labelCol="Cover_Type", featuresCol="featureVector",
predictionCol="prediction")

model = classifier.fit(assembled_train_data)
print(model.toDebugString)
...
DecisionTreeClassificationModel:
uid=DecisionTreeClassifier_da03f8ab5e28, depth=5, numNodes=41,
numClasses=8, numFeatures=54
If (feature 0 <= 3036.5)
If (feature 0 <= 2546.5)
If (feature 10 <= 0.5)
If (feature 0 <= 2412.5)
If (feature 3 <= 15.0)
Predict: 4.0
Else (feature 3 > 15.0)
Predict: 3.0
Else (feature 0 > 2412.5)
...
```

同样，分类器的基本配置由列名组成：包含输入特征向量的列和包含要预测的目标值的列。因为模型稍后将用于预测目标的新值，所以给它指定了一个用于存储预测结果的列名。

打印模型的表示形式会显示其部分树结构。它由一系列关于特征的嵌套决策组成，将特征值与阈值进行比较。（出于历史原因，不幸的是，这里的特征仅按编号引用，而不按名称。）

决策树能够在构建过程中评估输入特征的重要性。也就是说，它们可以估计每个输入特征

