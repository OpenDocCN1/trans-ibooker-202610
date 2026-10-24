

## 15 数据获取

你可以按如下方式选择一个工作表：

```python
currSheet = wb['Sheet1']
print(currSheet)
print(type(currSheet))
```

输出如下：

```text
<Worksheet "Sheet1">
<class 'openpyxl.worksheet.worksheet.Worksheet'>
```

同样，以下代码具有相同的效果：

```python
currSheet = wb[wb.sheetnames[0]]
print(currSheet)
print(type(currSheet))
```

你可以按如下方式打印当前工作表的名称：

```python
print(currSheet.title)
```

输出如下：

Sheet1

你可以按如下方式打印单元格的值：

```python
var1 = currSheet['A1']
print(var1.value)
```

输出如下：

Food Item

完成相同操作的另一种方法是如下所示：

```python
print(currSheet['B1'].value)
```

你也可以按如下方式进行：

```python
var2 = currSheet.cell(row=2, column=2)
print(var2.value)
```

行数和列数可以按如下方式获取：

```python
print(currSheet.max_row)
print(currSheet.max_column)
```

输出如下：

```text
11
3
```

让我们按如下方式打印电子表格中的所有数据：

```python
for i in range(currSheet.max_row):
    print('---Beginning of Row---')
    for j in range(currSheet.max_column):
        var = currSheet.cell(row=i+1, column=j+1)
        print(var.value)
    print('---End of Row---')
```

输出非常长，这里已截断。请自行运行代码查看。

### 使用 NumPy 读写文件

让我们看看如何使用 NumPy 读写文件。让我们按如下方式使用 NumPy 创建一个数据集：

```python
import numpy as np
x = np.arange(100)
print(x)
```

输出如下：

```text
[  0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
  24 25 26 27 28 29 30 31 38 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47
  48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71
  72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95
  96 97 98 99]
```

你可以按如下方式将其保存到文件中（NumPy 数据格式）：

```python
np.save('test.npy', x)
```

你可以按如下方式从文件中将数据加载到变量中：

```python
data = np.load('test.npy')
print(data)
```

输出如下：

```text
[ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47
 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71
 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95
 96 97 98 99]
```

### 使用 NumPy 从 CSV 文件读取数据

CSV 文件也可以使用 NumPy 按如下方式读取：

```python
import numpy as np
# Reads only numeric data
data = np.loadtxt('data.csv', delimiter=',')
print(data)
```

输出如下：

```text
[[ 0.   1.  18.   2.]
 [ 1.   6.   1.   3.]
 [ 2.   3. 154.   0.]
 [ 4. 978.   3.   6.]
 [ 5.   2.  41.  45.]
 [ 6.  67.   2.   3.]
 [ 7.   5.  67.   2.]]
```

你也可以按如下方式跳过行和列：

```python
data = np.loadtxt('data.csv', delimiter=',',
                  skiprows=3, usecols=[1, 3])
print(data)
```

输出如下：

```text
[[978.   6.]
 [  2.  45.]
 [ 67.   3.]
 [  5.   2.]]
```

### Matplotlib CBook

你可以读取存储在 Matplotlib CBook 格式中的数据。Matplotlib 附带了一些该格式的示例文件。让我们看看如何读取数据：

```python
import matplotlib.cbook as cbook
datafile = cbook.get_sample_data('aapl.npz')
r = np.load(datafile)
print(r.files)
```

这将打印数据文件的名称，如下所示：

```text
['price_data']
```

让我们从该数据文件中读取数据：

```python
print(r['price_data'])
```

这会显示 Apple 股价数据如下：

```text
[('1984-09-07', 26.5, 26.87, 26.25, 26.5, 2981600, 3.02)
 ('1984-09-10', 26.5, 26.62, 25.87, 26.37, 2346400, 3.01)
 ('1984-09-11', 26.62, 27.37, 26.62, 26.87, 5444000, 3.07) ...
 ('2008-10-10', 85.7, 100., 85., 96.8, 79260700, 96.8)
 ('2008-10-13', 104.55, 110.53, 101.02, 110.26, 54967000, 110.26)
 ('2008-10-14', 116.26, 116.4, 103.14, 104.08, 70749800, 104.08)]
```

### 从 CSV 读取数据

如前所述，CSV 文件包含由逗号分隔的值。你可以使用 Pandas 中多功能的 `read_csv()` 函数读取 Web 上的 CSV 文件或本地/网络磁盘上的 CSV 文件。以下是我们将在本演示中使用的 CSV 文件的内容：

```text
rank,discipline,phd,service,sex,salary
Prof,B,56,49,Male,186960
Prof,A,12,6,Male,93000
Prof,A,23,20,Male,110515
Prof,A,40,31,Male,131205
Prof,B,20,18,Male,104800
Prof,A,20,20,Male,122400
AssocProf,A,20,17,Male,81285
```

第一行是标题行。大多数 CSV 文件会有标题行，尽管不是必需的。如你所见，值由逗号分隔。这是 CSV 文件的常见格式。根据系统和应用程序，你可以使用空格、分号（;）或竖线（|）等各种分隔符。此外，CSV 文件可以使用固定数量的字符来存储列中的数据。在本示例中，如讨论所述，我们使用的是存储数据的最常见 CSV 格式之一。

让我们学习如何使用 Pandas 从此类文件读取数据。为本章创建一个新的笔记本。

按如下方式导入 Pandas 库：

```python
import pandas as pd
```

让我们按如下方式读取位于 Web 上的 CSV 文件：

```python
df1 = pd.read_csv("http://rcs.bu.edu/examples/python/data_analysis/Salaries.csv")
print(df1)
```

你也可以按如下方式读取存储在本地磁盘上的 CSV：

```python
df2 = pd.read_csv("Salaries.csv")
print(df2)
```

你也可以按如下方式将数据框的数据转储到磁盘位置的 CSV 文件中：

```python
df2.to_csv('output.csv', index=True, header=False)
```

代码将在当前目录的磁盘上创建 CSV 文件。

### 从 Excel 文件读取数据

要将数据从 Excel 文件读取到 Pandas 数据框中，你需要外部包的支持。让我们按如下方式安装一个包：

```bash
!pip3 install xlrd
```

现在让我们按如下方式读取存储在磁盘上的 Excel 文件：

```python
excel_file = 'test.xlsx'
df1 = pd.read_excel(excel_file)
```

这里（以及之前的示例中），文件存储在与笔记本文件相同的目录中。如果需要在其他位置读取文件，必须指定该文件的完整路径。执行前面的代码后，将加载 Excel 文件的内容到 Pandas 数据框中。你可以使用以下代码行查看内容：

```python
print(df1)
```

图 15-1 显示了输出。

|   | Fruit | Color | Weight |
|---|---|---|---|
| 0 | Banana | Yellow | 250 |
| 1 | Orange | Orange | 200 |
| 2 | Grapes | Green | 400 |
| 3 | Tomato | Red | 100 |
| 4 | Spinach | Green | 40 |
| 5 | Potatoes | Gray | 400 |
| 6 | Rice | White | 300 |
| 7 | Rice | Brown | 400 |
| 8 | Wheat | Brown | 500 |
| 9 | Barley | Yellow | 500 |

图 15-1. 来自 Excel 工作表的数据

### 从 JSON 读取数据

你可以按如下方式将 JSON 字符串的数据读取到数据框中。首先创建一个 JSON 字符串。

```python
obj = """
{"name": "Ashwin",
"places_lived": ["Nashik", "Hyderabad", "Bangalore"],
"pet": null,
"siblings": [{"name": "Scott", "age": 30, "pets": ["Zeus", "Zuko"]},
{"name": "Katie", "age": 38,
"pets": ["Sixes", "Stache", "Cisco"]}]
}
"""
```

你可以按如下方式打印字符串：

```python
print(obj)
```

你也可以按如下方式检查变量的类型（它是一个 JSON 格式的字符串）：

```python
print(type(obj))
```

你可以按如下方式将此 JSON 格式的字符串转换为字典：

```python
import json
result = json.loads(obj)
print(result)
```

让我们按如下方式检查新创建变量的数据类型：

```python
print(type(result))
```

这将产生以下结果：

```text
<class 'dict'>
```

让我们按如下方式将数据加载到数据框中：

```python
df1 = pd.DataFrame(result['siblings'], columns=['name', 'age'])
print(df1)
```

