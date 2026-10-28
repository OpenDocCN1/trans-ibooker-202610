

&lt;matplotlib_venn._common.VennDiagram at 0x227fb9a02e0&gt;
&lt;Figure size 432x288 with 1 Axes&gt;

```
plt.draw()
```

![](img/4ffeafa3e3f1373f02971e43b0755ea6_602_0.png)

### Exercise 14.29

“Grey”和“White”圆圈中的数字 18 和 27 分别是什么意思？

### 14.10 Summary

在本章中，我们学习了描述性统计的基本概念，并看到了如何使用 **numpy** 和 **matplotlib** 进行计算和可视化。然后我们转向了 **pandas**，这是一个用于读取、写入和分析 DataFrames 的高度复杂的 Python 包。我们重新回顾了统计学，以展示 **pandas** 如何轻松地让我们检查和汇总大型和小型数据集。

下一章将结束本书，但不包括附录、参考文献和索引。我将简要介绍使用 Python 进行机器学习。这方面的知识将使我希望你能够继续阅读更高级的书籍和文章，以了解人工智能 Python 工具的全部广度。

## 15 Learning, Briefly

> *Learning is the only thing the mind never exhausts, never fears, and never regrets.*
> 
> —Leonardo da Vinci

机器学习并非新事物，但它及其子学科深度学习现在被广泛用于人工智能（AI）的许多应用。有数百本关于机器学习的学术和实践编程书籍。

本最后一章主要通过 scikit-learn **sklearn** 模块介绍机器学习和神经网络。将此视为一个跳板，你可以在此基础上利用本书中学到的 Python 特性，更深入地探索这些重要的 AI 领域，如果它们引起你的兴趣。

### Topics covered in this chapter

- 15.1 What is machine learning? — 576
- 15.2 Cats again — 577
- 15.3 Feature scaling — 580
- 15.4 Feature selection and reduction — 586
- 15.5 Clustering — 589
- 15.6 Classification — 595
- 15.7 Linear regression — 605
- 15.8 Concepts of neural networks — 611
- 15.9 Quantum machine learning — 618
- 15.10 Summary — 619

### 15.1 What is machine learning?

考虑我与某在线零售商的完整产品浏览和购买历史。该数据代表了我从卖家处购买的体验，以及卖家向我销售的体验。数据和我的新购买如何帮助零售商向我销售额外的产品和服务？

机器学习是一组技术，其中计算机算法从现有数据中学习，而不是在其中内置硬编码决策。这种算法在接收到新数据时，其准确性会提高。它也可能在人类或流程介入以判断之前结果的正确性和质量时变得更好。

在 *无监督学习* 中，算法会在数据中寻找可能帮助用户获得洞察的模式。无监督算法可能帮助零售商看到我在某些季节对烹饪、旅行或家庭维修的兴趣有所增加。卖家随后可以在我通常购买之前几周就提前向我提供产品推荐。

在 *监督学习* 中，数据在其特征中还包含我们可能对其提出的问题的“答案”。假设在线零售商还拥有一个旅行社。我在购买旅行后购买了推荐的书籍和衣服。在这种情况下，零售商可以将书籍和衣服数据标记为“为旅行购买”。给定新的服装购买，零售商的机器学习算法可能会强烈建议我正在计划度假并提供可能的航班和酒店。

在 *强化学习* 中，算法会根据结果添加某种奖励或惩罚。如果我购买后退回产品，算法可能会在与推断结果相关的数据中插入一个负分作为特征。该特征可以帮助算法在两个可能的操作之间做出更好的未来选择。也许我正在旅行，想买新衬衫。如果我购买后退回其中一些，零售商可以更新数据以注明我不喜欢去温暖气候时穿有长尾和扣子的衬衫。

获取强化数据另一种方式是直接询问用户。如果零售商网站向你展示产品然后询问，“你喜欢这个推荐吗？”，卖家就可以使用“是”或“否”的回应来调整其推荐算法。当然，如果你购买了产品，这应该会产生积极的强化。

### Exercise 15.1

你如何使用强化学习来教一辆自动驾驶汽车在绕过道路弯道时应该行驶多快？

机器学习算法用来“学习”的信息称为 *训练集*。对于监督学习，训练集有一个或多个 *标签* 特征。当我们获得新的未标记数据时，我们运行算法并希望它与预期结果相匹配。当我们用算法 *拟合* 训练数据以进行预测时，我们创建了一个机器学习 *模型*。

测试机器学习算法的常见方法是随机选择所有标记数据的 50% 并将其指定为训练集。然后，取剩余的数据，忽略标签，看看算法在该 *测试集* 中预测结果的效果如何。尽可能调整算法以提高准确性。你可能需要更改使用的特征，归一化特征（第 15.3 节），减少特征数量（第 15.4 节），或完全切换到另一种技术。

### 15.2 Cats again

我们再次使用我在第 14.2 节中介绍的来自大丹宁根市的猫数据。如前所述，我们清理数据并创建性别虚拟列。

```python
import pandas as pd

df = pd.read_csv("src/examples/registered-cats.csv")
del df["Animal_Type"]
del df["Postcode"]

new_column_names = {
    "Breed_Description": "Breed",
    "Colour_Description": "Colour",
    "GENDER": "Gender"
}

df.rename(columns=new_column_names, inplace=True)

df.loc[611, ["Gender"]] = 'F'
df = df[df["Gender"].str.contains("F|M")]
df.reset_index(drop=True, inplace=True)
```

现在是地区：

```python
locality_dict = dict()

for count, locality in enumerate(np.sort(df["Locality"].unique())):
    locality_dict[locality] = count
```

现在我们准备更新条目。我遍历 DataFrame 行并将字符串品种、颜色和地区值作为元组提取出来。然后我查找 int 编码并将它们放入 DataFrame 中：

```python
for n in range(df.shape[0]):
    breed, colour, locality = df.loc[n, ["Breed", "Colour", "Locality"]]
    df.at[n, "Breed"] = breed_dict[breed]
    df.at[n, "Colour"] = colour_dict[colour]
    df.at[n, "Locality"] = locality_dict[locality]
```

注意我如何使用 at 来处理每一行和每一列中的单个值。

### Exercise 15.2

at 是使用索引还是数值行位置值？如果我告诉你还有一个名为 iat 的函数呢？

我们最后将 Breed、Colour 和 Locality 列类型转换为 int16：

```python
df = df.astype({"Breed": "int16", "Colour": "int16", "Locality": "int16"})
df.info()
```

&lt;class 'pandas.core.frame.DataFrame'&gt;
RangeIndex: 3475 entries, 0 to 3474
Data columns (total 5 columns):

| # | Column | Non-Null Count | Dtype |
|---|--------|----------------|-------|
| 0 | Locality | 3475 non-null | int16 |
| 1 | Breed | 3475 non-null | int16 |
| 2 | Colour | 3475 non-null | int16 |
| 3 | F | 3475 non-null | int8 |
| 4 | M | 3475 non-null | int8 |

dtypes: int16(3), int8(2)
memory usage: 27.3 KB

```python
df.head()
```

|  | Locality | Breed | Colour | F | M |
|---|---|---|---|---|---|
| 0 | 2 | 21 | 112 | 1 | 0 |
| 1 | 2 | 19 | 17 | 0 | 1 |
| 2 | 1 | 21 | 116 | 1 | 0 |
| 3 | 8 | 18 | 121 | 1 | 0 |
| 4 | 1 | 18 | 130 | 0 | 1 |

以下句子是等价的：

- df has 5 columns.
- df has 5 *features*.
- df has 5 *dimensions*.
- df is 5-dimensional.

由于我们将使用并看到许多浮点数，我现在使用 pandas 功能以标准、非科学记数法显示浮点数：

```python
pd.set_option("display.float_format", lambda x: "%.4f" % x)
```

格式表示法与我们在 f-string 中使用的类似，但来自 Python 历史上的更早时期。它表示，“将 x 替换为字符串中的浮点数，并显示小数点后 4 位数字。”我们仍会看到计算舍入误差，其中 Python 显示的数字在数学上本应为 0.0 和 1.0，但显示为接近但不等于这些值。

我们对 numpy 做类似处理：

```python
np_float_formatter = "{:.4f}".format
np.set_printoptions(formatter={'float_kind':np_float_formatter})
```

### 15.3 Feature scaling

由于我们现在有了维度，几何学进入画面也就顺理成章了。如果我在笛卡尔平面上有两个点，v = (v₁, v₂) 和 w = (w₁, w₂)，那么它们之间的欧几里得距离，或简单地说是距离，为

‖v − w‖ = √((v₁ − w₁)² + (v₂ − w₂)²)

