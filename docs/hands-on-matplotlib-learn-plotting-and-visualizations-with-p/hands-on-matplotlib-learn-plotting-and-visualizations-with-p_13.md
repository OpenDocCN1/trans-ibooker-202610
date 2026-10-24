# 第 14 章

# Pandas 入门

在上一章中，你学习了很多 Matplotlib 技术。现在你将学习如何使用另一个在数据科学和数据可视化中很常见的库。

在本章中，我们将重点介绍科学 Python 生态系统中主要的数据科学和分析库：Pandas 的基础知识。你将了解该库中的数据结构。以下是本章的主题：

- Pandas 入门
- Pandas 中的 Series
- Pandas 中的 Dataframe

读完本章后，你将能够熟练地使用 Pandas 完成基本任务。

## Pandas 简介

Pandas 是科学 Python 生态系统中的数据分析组件。事实上，它是科学 Python 生态系统中不可或缺的一部分。它提供了多种多样的数据结构和管理这些数据结构的例程。它提供了多种多样的数据结构和管理这些数据结构的过程。

让我们通过在 Jupyter Notebook 中运行以下命令来在计算机上安装 Pandas：

```
!pip3 install pandas
```

你可以通过运行以下命令将其导入当前会话：

```
import pandas as pd
```

你可以在 https://pandas.pydata.org/ 了解更多关于 Pandas 的信息。

## Pandas 中的 Series

Series 是一个带有标签的一维数组。它可以包含任何类型的数据。这些标签统称为*索引*。

你可以按如下方式创建一个 Series：

```python
s1 = pd.Series([3, 2, 1, 75, -3.14])
```

你可以按如下方式检查它的数据类型：

```python
type(s1)
```

输出如下：

```text
<class 'pandas.core.series.Series'>
```

你可以按如下方式查看与数据关联的值和索引：

```python
print(s1)
```

输出如下：

```text
0     3.00
1     2.00
2     1.00
3    75.00
4    -3.14
dtype: float64
```

你可以按如下方式显式指定数据类型：

```python
s2 = pd.Series([3, 2, 1, 75, -3.14], dtype=np.float32)
print(s2)
```

你可以将列表作为参数传递给构造函数来创建 Series，如下所示：

```python
x = [3, 2, 1, 75, -3.14]
s3 = pd.Series(x)
```

你甚至可以将 NumPy 的 Ndarray 作为参数传递给构造函数来创建 Series，如下所示：

```python
import numpy as np
y = np.array(x)
s4 = pd.Series(y)
```

你可以按如下方式查看值：

```python
print(s4.values)
```

输出如下：

```text
[ 3.    2.    1.   75.   -3.14]
```

你可以按如下方式检索索引：

```python
print(s4.index)
```

输出如下：

```text
RangeIndex(start=0, stop=5, step=1)
```

你可以按如下方式分配自定义索引：

```python
s5 = pd.Series( x, index = ['a', 'b', 'c', 'd', 'e'])
print(s5)
```

输出如下：

```text
a     3.00
b     2.00
c     1.00
d    75.00
e    -3.14
dtype: float64
```

## Series 的基本操作

你可以对 Series 执行一些基本操作。例如，你可以按如下方式显示负数：

```
print(s5[s5 < 0])
```

输出如下：

```
e    -3.14
dtype: float64
```

你可以按如下方式获取正数：

```
print(s5[s5 > 0])
```

输出如下：

```
a    3.0
b    2.0
c    1.0
d    75.0
dtype: float64
```

以上是比较操作的示例。你可以执行算术运算，例如乘法，如下所示：

```
c = 3
print ( s5 * c )
```

输出如下：

```
a      9.00
b      6.00
c      3.00
d    225.00
e     -9.42
dtype: float64
```

## Pandas 中的 Dataframe

Dataframe 是一种二维带标签的数据结构，其列可以是不同的数据类型。你可以从 Series、Ndarray、列表和字典创建 Dataframe。

Dataframe 具有标签，这些标签统称为*索引*。你可以轻松地查看和操作 Dataframe 中的数据。数据以矩形网格格式存储在 Dataframe 中。

你可以按如下方式从列表创建 Dataframe。以下是一个字典：

```
data = {'city': ['Delhi', 'Delhi', 'Delhi',
    'Bangalore', 'Bangalore', 'Bangalore'],
    'year': [2020, 2021, 2022, 2020, 2021, 2022,],
    'population': [10.0, 10.1, 10.2, 5.2, 5.3, 5.5]}
```

让我们由此创建一个 Dataframe，如下所示：

```
df1 = pd.DataFrame(data)
print(df1)
```

输出如下：

|    | 城市      | 年份 | 人口 |
|----|----------|------|------------|
| 0  | 德里     | 2020 | 10.0       |
| 1  | 德里     | 2021 | 10.1       |
| 2  | 德里     | 2022 | 10.2       |
| 3  | 班加罗尔 | 2020 | 5.2        |
| 4  | 班加罗尔 | 2021 | 5.3        |
| 5  | 班加罗尔 | 2022 | 5.5        |

你可以按如下方式查看前五条记录：

```
df1.head()
```

输出如下：

|    | 城市  | 年份 | 人口 |
|----|-------|------|------------|
| 0  | 德里 | 2020 | 10.0       |
| 1  | 德里 | 2021 | 10.1       |
| 2  | 德里 | 2022 | 10.2       |
| 3  | 班加罗尔 | 2020 | 5.2 |
| 4  | 班加罗尔 | 2021 | 5.3 |

你还可以将其他数字作为参数传递给 head() 函数，它将显示 Dataframe 中的相应数量的顶部记录。类似地，你可以使用 df1.tail() 查看末尾记录。它也有 5 作为默认参数，但你可以自定义传递给它的参数。

你可以按如下方式创建具有特定列顺序的 Dataframe：

```
df2 = pd.DataFrame(data, columns=['year', 'city', 'population'])
print(df2)
```

输出如下：

year    city    population
0   2020    Delhi   10.0
1   2021    Delhi   10.1
2   2022    Delhi   10.2
3   2020    Bangalore   5.2
4   2021    Bangalore   5.3
5   2022    Bangalore   5.5

让我们创建一个带有额外列和自定义索引的 Dataframe，如下所示：

```
df3 = pd.DataFrame(data, columns=['year', 'city', 'population', 'GDP'],
                index = ['one', 'two', 'three', 'four', 'five', 'six'])
print(df3)
```

以下是新的 Dataframe：

year    city    population  GDP
one 2020    Delhi   10.0    NaN
two 2021    Delhi   10.1    NaN
three   2022    Delhi   10.2    NaN
four    2020    Bangalore   5.2 NaN
five    2021    Bangalore   5.3 NaN
six 2022    Bangalore   5.5 NaN

你可以按如下方式打印列列表：

```
print(df3.columns)
```

输出如下：

```
Index(['year', 'city', 'population', 'GDP'], dtype='object')
```

你可以按如下方式打印索引列表：

```
print(df3.index)
```

输出如下：

```
Index(['one', 'two', 'three', 'four', 'five', 'six'], dtype='object')
```

你可以通过以下语句查看某一列的数据：

```
print(df3.year)
```

或者你也可以使用以下语句：

```
print(df3['year'])
```

输出如下：

```
one      2020
two      2021
three    2022
four     2020
five     2021
six      2022
Name: year, dtype: int64
```

你可以通过以下语句查看某一列的数据类型：

```
print(df3['year'].dtype)
```

或者你可以使用以下方式：

```
print(df3.year.dtype)
```

输出如下：

```
int64
```

你可以按如下方式查看所有列的数据类型：



print(df3.dtypes)

输出如下：

```
year             int64
city            object
population     float64
GDP             object
dtype: object
```

你可以使用索引按如下方式检索任何记录：

```python
df3.loc['one']
```

输出如下：

```
year              2020
city              Delhi
population        10.0
GDP               NaN
Name: one, dtype: object
```

你可以按如下方式将相同的值赋给列的所有成员：

```python
df3.GDP = 10
print(df3)
```

输出如下：

|       | year | city     | population | GDP |
|-------|------|----------|------------|-----|
| one   | 2020 | Delhi    | 10.0       | 10  |
| two   | 2021 | Delhi    | 10.1       | 10  |
| three | 2022 | Delhi    | 10.2       | 10  |
| four  | 2020 | Bangalore| 5.2        | 10  |
| five  | 2021 | Bangalore| 5.3        | 10  |
| six   | 2022 | Bangalore| 5.5        | 10  |

你可以按如下方式将 Ndarray 分配给列 GDP：

```python
import numpy as np
df3.GDP = np.arange(6)
print(df3)
```

输出如下：

|     | year | city     | population | GDP |
| --- | ---- | -------- | ---------- | --- |
| one   | 2020 | Delhi      | 10.0 | 0   |
| two   | 2021 | Delhi      | 10.1 | 1   |
| three | 2022 | Delhi      | 10.2 | 2   |
| four  | 2020 | Bangalore  | 5.2  | 3   |
| five  | 2021 | Bangalore  | 5.3  | 4   |
| six   | 2022 | Bangalore  | 5.5  | 5   |

你也可以按如下方式为其分配一个列表：

```python
df3.GDP = [3, 2, 0, 9, -0.4, 7]
print(df3)
```

输出如下：

|     | year | city     | population | GDP |
| --- | ---- | -------- | ---------- | --- |
| one   | 2020 | Delhi      | 10.0 | 3.0 |
| two   | 2021 | Delhi      | 10.1 | 2.0 |
| three | 2022 | Delhi      | 10.2 | 0.0 |
| four  | 2020 | Bangalore  | 5.2  | 9.0 |
| five  | 2021 | Bangalore  | 5.3  | -0.4|
| six   | 2022 | Bangalore  | 5.5  | 7.0 |

让我们按如下方式为其分配一个 series：

```python
val = pd.Series([-1.4, 1.5, -1.3], index=['two', 'four', 'five'])
df3.GDP = val
print(df3)
```

输出如下：

|     | year | city     | population | GDP |
| --- | ---- | -------- | ---------- | --- |
| one   | 2020 | Delhi      | 10.0 | NaN |
| two   | 2021 | Delhi      | 10.1 | -1.4|
| three | 2022 | Delhi      | 10.2 | NaN |
| four  | 2020 | Bangalore  | 5.2  | 1.5 |
| five  | 2021 | Bangalore  | 5.3  | -1.3|
| six   | 2022 | Bangalore  | 5.5  | NaN |

## Summary

在本章中，你探索了科学 Python 生态系统中 Pandas 数据科学库的基础。你学习了创建和使用 Pandas 基本数据结构的基础知识，即 series 和 dataframe。

在下一章中，你将学习如何使用 NumPy、Pandas 和 Matplotlib 库以编程方式读取以各种格式存储的数据。
