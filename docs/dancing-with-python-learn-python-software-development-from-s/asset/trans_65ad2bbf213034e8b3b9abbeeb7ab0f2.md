

# Column          Non-Null Count  Dtype
---  ------        --------------  -----
0   Locality       3485 non-null   object
1   Postcode       3485 non-null   int64
2   Animal_Type    3485 non-null   object
3   Breed_Description 3482 non-null object
4   Colour_Description 3483 non-null object
5   GENDER         3485 non-null   object
dtypes: int64(1), object(5)
memory usage: 163.5+ KB

此显示告诉我们新的 DataFrame 具有

+   - 六列和 3,485 条记录
+   - Postcode 字段以 **numpy** 64 位整数加载（dtype **int64**）
+   - Locality、Animal_Type、Breed_Description、Colour_Description 和 GENDER 的 dtype **object** 字段

就我们的目的而言，我们可以将对象视为字符串。

虽然 Python **int** 数字可以有任意数量的位数，但 **int8**、**int16**、**int32** 和 **int64** 类型对象分别使用 8、16、32 和 64 位。在 **numpy** 和 **pandas** 中使用时，使用能安全容纳你的值且最小的尺寸。

*head* 函数默认显示 DataFrame 中的前五行记录。给它一个正整数参数以显示更多或更少。

```
df.head()
```

|   | Locality | Postcode | Animal_Type | Breed_Description | Colour_Description |
|---:|---|---:|---|---|---|
| 0 | DANDENONG NORTH | 3175 | Cat | DOMSH | TAB |
| 1 | DANDENONG NORTH | 3175 | Cat | DOMLH | BLAWHI |
| 2 | DANDENONG | 3175 | Cat | DOMSH | TABWHI |
| 3 | SPRINGVALE | 3171 | Cat | DOM | TORWHI |
| 4 | DANDENONG | 3175 | Cat | DOM | WHIGRE |

|   | GENDER |
|---:|---|
| 0 | F |
| 1 | M |
| 2 | F |
| 3 | F |
| 4 | M |

tail 函数默认显示 DataFrame 中的最后五行记录。

```
df.tail()
```

Locality Postcode Animal_Type Breed_Description Colour_Description \
3480 SPRINGVALE 3171 Cat RAG SEAL
3481 NOBLE PARK 3174 Cat DOMM BLACK
3482 NOBLE PARK 3174 Cat DOMM WHIGIN
3483 SPRINGVALE 3171 Cat RAGX BLUEP
3484 SPRINGVALE 3171 Cat DOMSH GINGER

GENDER
3480 M
3481 F
3482 M
3483 M
3484 M

该显示相当混乱。让我们去掉多余的数据并改进列名。

## 14.3.2 Changing columns

在第 3.2.4 节中，我们看到了如何使用 del 删除列表项。我们可以用它从 DataFrame 中删除一列。Animal_Type 列是多余的，因为我们知道每只动物都是猫。让我们将其删除：

```
del df["Animal_Type"]
```

```
df.head()
```

Locality Postcode Breed_Description Colour_Description GENDER
0 DANDENONG NORTH 3175 DOMSH TAB F
1 DANDENONG NORTH 3175 DOMLH BLAWHI M
2 DANDENONG 3175 DOMSH TABWHI F
3 SPRINGVALE 3171 DOM TORWHI F
4 DANDENONG 3175 DOM WHIGRE M

显示更简洁了，但我们仍然可以缩短 Breed_Description 和 Colour_Description 列标签。此外，GENDER 列为大写，而其他列都不是，这很奇怪。

现在我们可以继续查看数据本身了。顺便说一下，在处理这个 DataFrame 时，我使用澳大利亚英语拼写“colour”而不是我本来的美国英语拼写“color”。

## 14.3.3 Examining a DataFrame

DataFrame 的 *shape* 属性返回行数和列数的元组。请注意，这是一个属性而不是方法，因此我们不使用括号。

```
```python
df.shape
```
```

(3485, 5)

这个 DataFrame 有超过三千行，但数据科学家经常使用有数十万或数百万行的 DataFrame。**pandas** 包有很多函数可以让你总结行和列的内容。例如，*nunique* 函数返回每列中唯一项的数量。

```
```python
df.nunique()
```
```

```
```text
Locality     10
Postcode      6
Breed        53
Colour      137
Gender        3
dtype: int64
```
```

正如你所见，数据中地区、邮编和性别相对较少，但品种和颜色很多。

*value_counts* 函数告诉你每列中每个唯一项出现的次数。

```
```python
df["Locality"].value_counts()
```
```

```
```text
NOBLE PARK          737
KEYSBOROUGH         656
DANDENONG NORTH     651
DANDENONG           549
SPRINGVALE          333
SPRINGVALE SOUTH    263
NOBLE PARK NORTH    224
BANGHOLME            68
DANDENONG SOUTH       3
LYNDHURST             1
Name: Locality, dtype: int64
```
```

## Analyzing Data

如果你使用 `normalize=True` 关键字参数，_value_counts_ 会以 _frequency_ 的形式给出结果，即百分比除以 100。

```
df['Locality'].value_counts(normalize=True)

NOBLE PARK             0.211478
KEYSBOROUGH            0.188235
DANDENONG NORTH        0.186801
DANDENONG              0.157532
SPRINGVALE             0.095552
SPRINGVALE SOUTH       0.075466
NOBLE PARK NORTH       0.064275
BANGHOLME              0.019512
DANDENONG SOUTH        0.000861
LYNDHURST              0.000287
Name: Locality, dtype: float64
```

_value_counts_ 默认按降序对结果排序。使用 `ascending=False` 反转该顺序，并使用 `sort=False` 关闭排序。

此 _series_ 中的索引是 BANGHOLME 和 NOBLE PARK NORTH 等地区名称。虽然 DataFrame 是 2 维的，但 series 是 1 维的。例如，我们可以提取一列作为 series。使用 `sort_index` 方法按其索引值对 series 进行排序。

```
cat_data = df["Locality"].value_counts().sort_index()
cat_data

BANGHOLME              68
DANDENONG             549
DANDENONG NORTH       651
DANDENONG SOUTH         3
KEYSBOROUGH            656
LYNDHURST                1
NOBLE PARK             737
NOBLE PARK NORTH       224
SPRINGVALE             333
SPRINGVALE SOUTH       263
Name: Locality, dtype: int64
```

> **Exercise 14.7**
> 
> `ascending` 和 `inplace` 关键字参数如何改变 `sort_index` 的行为？

## Exercise 14.8

DataFrame 中有多少不同的品种和颜色，它们是什么？

为了更好地理解这些数据，我们现在创建一个条形图。我不会重复第 13.2 节中关于 matplotlib 条形图的讨论；但会向你展示如何使用 pandas 及其对应的可视化函数。

给定一个 pandas DataFrame 或 series 对象，使用带有 kind="bar" 关键字参数的 plot 来创建简单的条形图。

```
import matplotlib.pyplot as plt
plt.clf()
cat_data.plot(kind="bar")
```

<AxesSubplot:>
<Figure size 432x288 with 1 Axes>

```
plt.draw()
```

![](img/4ffeafa3e3f1373f02971e43b0755ea6_573_0.png)

我们也可以使用 cat_data.plot.bar()。

现在我们美化我们的图表。首先，我们将水平轴标签旋转 45 度，因为它们太长了。我们还使条形为浅灰色，带黑色轮廓和斜线阴影。

```
plt.clf()
```

```
cat_data.plot(kind="bar", color="lightgray", edgecolor="black", rot=45, hatch="///")
```

<AxesSubplot:>
<Figure size 432x288 with 1 Axes>

水平标签仍然很大，所以我们减小其字体大小。

```
plt.tick_params(axis='x', labelsize=7)
```

<Figure size 432x288 with 1 Axes>

最后，我们为坐标轴添加标签和图表标题。

```
plt.xlabel("Locality")
plt.ylabel("Number of cats")
plt.title("Number of registered cats in 2021 per locality in the City of Greater Dandenong")
```

Text(0.5, 1.0, 'Number of registered cats in 2021 per locality in the City of Greater Dandenong')
<Figure size 432x288 with 1 Axes>

```
plt.draw()
```

![](img/4ffeafa3e3f1373f02971e43b0755ea6_574_0.png)

pandas 文档包含其可视化能力的完整指南。 [P.V.Z] 如示例所示，大多数都基于你在 matplotlib 中可以完成的功能。

`small_df` 中的第一行索引为 50，尽管其整数位置为 0。

```
```python
small_df.loc[50]
```
```

```text
Locality      SPRINGVALE SOUTH
Postcode                  3172
Breed                     DOMSH
Colour                   BLAWHI
Gender                        F
Name: 50, dtype: object
```

如果你混淆索引和整数位置，你很可能会得到错误。

```
```python
small_df.loc[0]
```
```

```
```text
ValueError: 0 is not in range
```
```

```
```python
small_df.iloc[50]
```
```

```
```text
IndexError: single positional indexer is out-of-bounds
```
```

要删除一个或多个行，请使用 *drop* 方法并传入行索引的列表。像往常一样，指定 `inplace=True` 会改变 DataFrame 本身而不是制作副本。

```
```python
small_df.drop([51], axis=0, inplace=True)
small_df
```
```

```text
   Locality       Postcode  Breed  Colour  Gender
50 SPRINGVALE SOUTH    3172  DOMSH  BLAWHI       F
52 SPRINGVALE SOUTH    3172  DOMSH  REDCRM       M
53 DANDENONG           3175  DOMM   BLAWHI       F
54 DANDENONG           3175  DOMLH  GREWHI       F
55 DANDENONG           3175  CHIN   WHISIL       M
56 DANDENONG           3175  DOMSH  TRI          F
```

DataFrame 有两个 *axes*，不要与 **matplotlib** 绘图轴混淆。Axis 0 指的是行，axis 1 是列。在 *drop* 的情况下，默认轴是 0，但我为了清晰起见包含它。你也可以使用 `axis="index"` 和 `axis="columns"` 代替 `axis=0` 和 `axis=1`。

```
```python
small_df.drop([53, 54], axis="index")
```
```

```text
   Locality       Postcode  Breed  Colour  Gender
50 SPRINGVALE SOUTH    3172  DOMSH  BLAWHI       F
```

