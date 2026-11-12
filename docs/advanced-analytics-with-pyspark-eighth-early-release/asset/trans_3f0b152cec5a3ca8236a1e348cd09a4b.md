

决策树算法负责在每一层尝试潜在的决策规则，如宠物店示例中的 `weight >= 100` 或 `weight >= 500` 决策。决策始终采用相同的形式：对于数值特征，决策的形式为 `feature >= value`；对于分类特征，则采用 `feature in (value1, value2, ...)` 形式。因此，要尝试的决策规则集实际上是一组要代入决策规则的值。这些在 PySpark MLlib 实现中被称为“bins”。更大的 bins 数量需要更多处理时间，但可能会找到更优的决策规则。

什么是好的决策规则？直觉上，好的规则能够按目标类别值有意义地区分示例。例如，将 Covtype 数据集分为一侧仅包含类别 1–3、另一侧仅包含类别 4–7 的规则将非常出色，因为它清楚地将某些类别与其他类别分开。如果一条规则导致所有类别的混合情况与整个数据集中发现的大致相同，那似乎没什么帮助。遵循此类决策的任何分支都会得到大致相同的可能目标值分布，因此不会真正向自信分类取得进展。

换句话说，好的规则将训练数据的目标值划分为相对均匀或“纯净”的子集。选择最佳规则意味着最小化其诱导的两个子集的不纯度。常用的不纯度度量有两种：Gini 不纯度和熵。

Gini 不纯度与随机猜测分类器的准确度直接相关。在子集内，它是随机选择一个示例并随机对其进行分类（两者均按子集中类别的分布进行），该分类 *错误* 的概率。这是类别比例乘积之和，但需减去它们自身，并从 1 中减去。如果子集包含 N 个类，p_i 为类 i 示例的比例，则其 Gini 不纯度由 Gini 不纯度公式给出：

```
\( I_G (p) = 1 - \sum_{i=1}^N p_i^2 \)
```

如果子集只包含一个类，该值为 0，因为它是完全“纯净”的。当子集中有 \(N\) 个类时，该值大于 0，并且当各类出现次数相同（即最不纯）时达到最大。

熵是另一种不纯度度量，借用自信息论。其本质更难解释，但它捕捉了子集中目标值集合对落在该子集中的数据的预测所暗示的不确定性程度。仅包含一个类的子集表明该子集的结果完全确定且熵为 0——没有不确定性。另一方面，包含每个可能类各一个的子集表明该子集的预测存在大量不确定性，因为已观察到各种目标值。这具有高熵。因此，低熵（如同低 Gini 不纯度）是好事。熵由熵方程定义：

![](img/c523d33c8cf752e889378f812513252c_114_0.png)

> 有趣的是，不确定性有单位。因为对数是自然对数（base \(e\)），单位是 *纳特*，即更熟悉的 *比特* 的 base-\(e\) 对应物（我们可以使用 log base 2 得到比特）。它确实在衡量信息，因此在将熵与决策树一起使用时，也常讨论决策规则的 *信息增益*。

在给定数据集中，两种不纯度度量中一种可能是选择决策规则的更好指标。从某种程度上说，它们相似。两者都涉及加权平均：按 \(p_i\) 加权的求和。PySpark 实现中的默认值是 Gini 不纯度。

最后，最小信息增益是一个超参数，它对候选决策规则施加最小信息增益（或不纯度下降）。改进子集不纯度不足的规则会被拒绝。与较低的最大深度类似，这有助于模型抵御过拟合，因为几乎无法帮助划分训练输入的决策，实际上可能根本无法有效划分未来数据。

既然我们了解了决策树算法的相关超参数，将在下一节中调优模型以提高其性能。

## 调优决策树

从数据中看出哪种不纯度度量能带来更好的准确度，或者哪个最大深度或 bins 数量足够而不过度，并非显而易见。幸运的是，如第 3 章一样，让 PySpark 尝试这些值的多种组合并报告结果非常简单。

首先，有必要设置一个封装前面部分执行的两个步骤的管道——创建特征向量并使用它来创建决策树模型。创建 VectorAssembler 和 DecisionTreeClassifier，并将这两个 Transformer 链接在一起，将产生一个 Pipeline 对象，将这两项操作表示为一项操作：

```
from pyspark.ml import Pipeline

assembler = VectorAssembler(inputCols=input_cols,
outputCol="featureVector")
classifier = DecisionTreeClassifier(seed=1234,
labelCol="Cover_Type", featuresCol="featureVector",
predictionCol="prediction")

pipeline = Pipeline(stages=[assembler, classifier])
```

当然，管道可以更长、更复杂。这就已经足够简单了。现在，我们还可以使用 PySpark ML API 的内置支持 ParamGridBuilder 来定义应测试的超参数组合。也是时候定义用于选择“最佳”超参数的评估指标了，这里同样是 MulticlassClassificationEvaluator。

```
from pyspark.ml.tuning import ParamGridBuilder

paramGrid = ParamGridBuilder(). \
addGrid(classifier.impurity, ["gini", "entropy"]). \
addGrid(classifier.maxDepth, [1, 20]). \
addGrid(classifier.maxBins, [40, 300]). \
addGrid(classifier.minInfoGain, [0.0, 0.05]). \
build()
multiclassEval = MulticlassClassificationEvaluator() \
    .setLabelCol("Cover_Type") \
    .setPredictionCol("prediction") \
    .setMetricName("accuracy")
```

这意味着将对四个超参数各取两个值来构建和评估模型，共 16 个模型。它们将按多类准确度进行评估。最后，TrainValidationSplit 将这些组件结合在一起——创建模型的管道、模型评估指标以及要尝试的超参数——并可以在训练数据上运行评估。值得注意的是，也可以在此处使用 CrossValidator 来执行完整的 k 折交叉验证，但它的开销是 k 倍，在大数据环境下增加的价值不大。因此，这里使用 TrainValidationSplit。

```
from pyspark.ml.tuning import TrainValidationSplit

validator = TrainValidationSplit(seed=1234,
estimator=pipeline,
evaluator=multiclassEval,
estimatorParamMaps=paramGrid,
trainRatio=0.9)

validator_model = validator.fit(train_data)
```

这将需要几分钟或更长时间，具体取决于您的硬件，因为它正在构建和评估许多模型。请注意，train ratio 参数设置为 0.9。这意味着训练数据实际上由 TrainValidationSplit 进一步细分为 90%/10% 子集。前者用于训练每个模型。剩余的 10% 输入被留作交叉验证集以评估模型。如果已经留出一些数据用于评估，那么为什么还要将原始数据的 10% 留作测试集？

如果 CV 集的目的是评估与训练集拟合的参数，那么测试集的目的是评估与 CV 集“拟合”的超参数。也就是说，测试集确保对最终选定模型及其超参数的准确度进行无偏估计。

假设此过程选定的最佳模型在 CV 集上表现出 90% 的准确度。合理地预期它在未来数据上也会表现出 90% 的准确度。然而，构建这些模型中存在随机性。偶然，此模型及其评估结果可能异常出色。最佳模型和评估结果可能受益于一点运气，因此其准确度估计可能略显乐观。换句话说，超参数也可能过拟合。

为了真正评估此最佳模型在未来示例上的表现可能如何，我们需要在未用于训练的示例上对其进行评估。但我们还需要避免使用 CV 集中用于评估的示例。这就是为什么要留出第三个子集（即测试集）的原因。

验证器的结果包含其找到的最佳模型。这本身也是其找到的最佳整体 _管道_ 的表示，因为我们提供了一个管道实例来运行。为了查询 DecisionTreeClassifier 选择的参数，需要从结果的 PipelineModel 中手动提取 DecisionTreeClassificationModel，它是管道中的最后一个阶段。

```
from pprint import pprint

best_model = validator_model.bestModel
pprint(best_model.stages[1].extractParamMap())

...
{Param(...name='predictionCol', doc='prediction column name.'):
'prediction',
 Param(...name='probabilityCol', doc='...'): 'probability',
 [...]
 Param(...name='impurity', doc='...'): 'entropy',
 Param(...name='maxDepth', doc='...'): 20,
 Param(...name='minInfoGain', doc='...'): 0.0,
 [...]
 Param(...name='featuresCol', doc='features column name.'):
 'featureVector',
 Param(...name='maxBins', doc='...'): 40,
 [...]
 Param(...name='labelCol', doc='label column name.'): 'Cover_Type'}
...
```

