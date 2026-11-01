
例如 `pivot`、`unpivot` 等等。

## 使用 Pandas 展平嵌套数据

Pandas 提供了通过删除不需要的列、重命名列以及根据需要更改索引来构建数据的功能。这有助于将数据格式更改为更标准、适合每种用例的格式。此外，Pandas 中还有一些函数可用于展平单列中的嵌套数据，以便更好地分析数据。

为了演示我们的工作，让我们考虑一个示例体育场数据，其中包含体育场的地址和餐饮选择。我们在示例数据集中没有该数据，因此将使用示例值来展示 JSON 数据展平的工作方式。我们将使用 `json_normalize` ([https://pandas.pydata.org/docs/reference/api/pandas.json_normalize.html](https://pandas.pydata.org/docs/reference/api/pandas.json_normalize.html)) 函数将嵌套的 JSON 数据展平成列以进行分析。现在我们可以用这种数据更高效地查询列。你可以通过按 **Location.State** 列进行筛选来查询加利福尼亚州有多少个体育场。

```python
stadium_address = {'Name':'Angel Stadium', 'Location':{'City':'Anaheim','Zipcode':'92806','State':'CA','Address':{'Street Name':'E Gene Autry Way','Number':'2000'}}, 'Dining':['The Sweet shop','Big A Burger']}
```

```python
from pandas import json_normalize
flat1=json_normalize(stadium_address)
flat1
```

| Name | Dining | Location.City | Location.Zipcode | Location.State | Location.Address.Street Name | Location.Address.Number |
|---|---|---|---|---|---|---|
| Angel Stadium | [The Sweet shop, Big A Burger] | Anaheim | 92806 | CA | E Gene Autry Way | 2000 |

图 9.30：使用 json_normalize 函数展平分层 JSON 数据

然而，**Dining** 列中仍然有列表数据。如果我们希望扩展该列，将有助于进一步分析。为此，我们需要创建一个函数来展平 JSON 字符串中的字典和列表数据。

```
#Function referenced from https://towardsdatascience.com/flattening-json-objects-in-python-f5343c794b10
def flatten_json(y):
    out = {}
    def flatten(x, name=''):
        if type(x) is dict:
            for a in x:
                flatten(x[a], name + a + '_')
        elif type(x) is list:
            i = 0
            for a in x:
                flatten(a, name + str(i) + '_')
                i += 1
        else:
            out[name[:-1]] = x
    flatten(y)
    return out
```

图 9.31：用于展平 JSON 数据中列表和字典列的函数

现在，你可以看到输出中字典和列表数据都已从 JSON 字符串展平。我们使用了前一步创建的 flatten_json 函数来实现所需输出：

```
flat = flatten_json(stadium_address)
json_normalize(flat)
```

| Name | Location_City | Location_Zipcode | Location_State | Location_Address_Street Name | Location_Address_Number | Dining_0 | Dining_1 |
|---|---|---|---|---|---|---|
| 0 Angel Stadium | Anaheim | 92806 | CA | E Gene Autry Way | 2000 | The Sweet shop | Big A Burger |

图 9.32：展平包含列表和字典列的 JSON 数据

让我们花点时间了解该函数的工作原理，以便你能进一步开发它。我们将为函数添加注释并演示 flatten 函数的工作方式。该函数将递归调用以展平 JSON 字符串：

```python
#For learning the working of the function.
def flatten(x, name='',out={}):
    print(x)
    a=""
    if type(x) is dict:
        for a in x:
            print("___________________")
            print("Dictionary route " + a)
            flatten(x[a], name + a + '_')
    elif type(x) is list:
        i = 0
        for a in x:
            print("*******************")
            print("List route " + name + str(i) + '_')
            flatten(a, name + str(i) + '_')
            i += 1
    else:
        print("====> Found a value for target column --> "+ x)
        out[name[:-1]] = x
```

图 9.33：通过添加注释了解 flatten 函数的工作原理

我们在函数中立即添加了 print 语句，以了解正在传递的输入字符串。我们在每个部分都添加了 print 语句，这些语句执行以下功能：

- 展平字典对象
- 展平列表对象
- 识别要存储在目标列中的原子值

### 函数的工作原理是什么？

该函数将遍历提供的数据并识别当前对象是字典还是列表：

- 字典对象将具有大括号并具有键值对结构，例如：{ "name":"Angel stadium", "state":"CA" }。字典内部可以包含字典或列表对象。
- 列表对象将具有重复多次的相似数据项。它们存储在大括号结构中。它们可用于存储多值属性。在前面的示例中，我们将 Angel 体育场内多个餐厅的数据存储在列表中——例如：["The sweet shop", "Big A burger"]。

一旦识别出对象，如果它是另一个对象内的字典或列表，它将递归调用该函数。

以下屏幕截图说明了该函数在提供最终输出之前如何解析数据。逐步跟进，你可以更好地理解该函数的工作原理：

```
flatten(stadium_address)
{'Name': 'Angel Stadium', 'Location': {'City': 'Anaheim', 'Zipcode': '92806', 'State': 'CA', 'Address': {'Street Name': 'E Gene Autry Way', 'Number': '2000'}}, 'Dining': ['The Sweet shop', 'Big A Burger']}
Dictionary route Name
Angel Stadium
====> Found a value for target column --> Angel Stadium
Dictionary route Location
{'City': 'Anaheim', 'Zipcode': '92806', 'State': 'CA', 'Address': {'Street Name': 'E Gene Autry Way', 'Number': '2000'}}
Dictionary route City
Anaheim
====> Found a value for target column --> Anaheim
Dictionary route Zipcode
92806
====> Found a value for target column --> 92806
Dictionary route State
CA
====> Found a value for target column --> CA
Dictionary route Address
{'Street Name': 'E Gene Autry Way', 'Number': '2000'}
Dictionary route Street Name
E Gene Autry Way
====> Found a value for target column --> E Gene Autry Way
Dictionary route Number
2000
====> Found a value for target column --> 2000
Dictionary route Dining
['The Sweet shop', 'Big A Burger']
********************
List route Dining_0_
The Sweet shop
====> Found a value for target column --> The Sweet shop
********************
List route Dining_1_
Big A Burger
====> Found a value for target column --> Big A Burger
```

图 9.34：JSON 展平函数的内部工作原理

现在，我们已成功将数据展平为单独的列以便更好地分析。然而，列表项在同一表中被拆分为多个列，这可能会创建宽列表和稀疏数据，这不利于查询数据。更好的方法是将列表中的数据拆分为具有外键关系的单独表。

为了实现这一点，我们将进一步更改 flatten 函数。以下是对 flatten 函数所做的更改摘要：

- 添加逻辑来处理数据集的主键，以便主键可以存储在子表中以便将它们连接起来。
- 我们将更改列表对象的展平逻辑以创建不同的 DataFrame。该逻辑还将填充主键迭代器列以识别原始列表对象中的不同值。
- 现在我们的函数将返回多个字典项，因此我们将更改函数输出以返回字典/列表对象的列表。

```
#For learning the working of the function.
def flatten_v2(x, name='',df_name="df",pk="Name",pk_val="",out={},out_dfs={}):
    print("Input -->",x, "--", df_name,"--",name)
    a=""
    df1={}
    df_list=[]
    if pk_val=="":
        pk_val=x[pk]
    if type(x) is dict:
        for a in x:
            print("___________________________")
            print("Dictionary route " + a)
            flatten(x[a], name + a + '_',"df",pk_val=pk_val)
    elif type(x) is list:
        i = 0
        for a in x:
            df1={}
            print("***************************")
            print("List route " + name + str(i) + '_')
            df_name=name[:-1]
            df1[name[:-1]]=a
            df1[name[:-1]+"_pk"]=i
            df1[pk]=pk_val
            df_list.append(df1)
            print("Output in list-->",df_list)
            i += 1
        out_dfs[df_name]=df_list
        return out_dfs
    else:
        print("====> Found a value for target column --> "+ x)
        out[name[:-1]] = x
    out_dfs[df_name]=out
    print("Output-->",out)
    return out_dfs
```

图 9.35：更新的 flatten 函数，用于将列表对象拆分为具有主键外键关系的另一个 DataFrame
