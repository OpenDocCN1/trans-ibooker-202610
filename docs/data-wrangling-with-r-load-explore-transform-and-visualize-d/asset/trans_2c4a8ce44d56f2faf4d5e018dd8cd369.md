
探索该函数，您可以在自己的环境中执行它以更好地理解其内部工作原理。现在让我们将其打包成一个可用的 flatten 函数并验证输出：

```
def flatten_json_v2(y):
    out = {}
    out_dfs= {}
    def flatten(x, name='',df_name="df",pk="Name",pk_val=""):
        a=""
        df1={}
        df_list=[]
        if pk_val=="":
            pk_val=x[pk]
        if type(x) is dict:
            for a in x:
                flatten(x[a], name + a + '_',"df",pk_val=pk_val)
        elif type(x) is list:
            i = 0
            for a in x:
                df1={}
                df_name=name[:-1]
                df1[name[:-1]]=a
                df1[name[:-1]+"_pk"]=i
                df1[pk]=pk_val
                df_list.append(df1)
                i += 1
            out_dfs[df_name]=df_list
            return out_dfs
        else:
            out[name[:-1]] = x
            out_dfs[df_name]=out
    flatten(y)
    return out_dfs
```

图 9.36：作为 flatten 函数的版本 2 创建的更新后的展平代码

在此处为我们的示例数据集执行该函数，我们现在可以看到该函数创建了两个 DataFrame，其中一个 DataFrame 包含来自列表对象的內容以及主键值（在此示例用例中为体育场名称），用于连接目的。

```
flat = flatten_json_v2(stadium_address)
json_normalize(flat['df'])
```

|   | Name | Location_City | Location_Zipcode | Location_State | Location_Address_Street Name | Location_Address_Number |
|---|------|---------------|------------------|----------------|------------------------------|-------------------------|
| 0 | Angel Stadium | Anaheim | 92806 | CA | E Gene Autry Way | 2000 |

```
json_normalize(test['Dining'])
```

|   | Dining | Dining_pk | Name |
|---|--------|-----------|------|
| 0 | The Sweet shop | 0 | Angel Stadium |
| 1 | Big A Burger | 1 | Angel Stadium |

图 9.37：验证更新后的 flatten 函数的工作原理

我们通过此练习展示了使用 pandas 和 Python 可以执行强大的数据转换以满足我们的需求。然而，这一逻辑可以进一步开发和微调。

## 挑战
您能否还更新脚本以处理嵌套列表？一个例子可以是体育场有一个餐饮店列表，而每个餐饮店都有一个菜单项列表。

## 提示
您可以尝试为层次结构中的每个表单独存储主键，然后将其与该表特定的迭代器列一起添加到子表中。

在下一节中，将了解更多关于使用 pandas’ 操作执行数据清洗活动的内容。

## 数据清洗
数据清洗是数据整理过程中的重要步骤。花费大量时间来识别正确的数据源并清洗数据。Pandas 提供了许多用于清洗数据的功能。

此阶段所需的确切活动因数据集类型而异。某些数据源的数据可能只需要最少量的清洗，而某些其他数据源可能需要在项目中使用数据集之前进行大量清洗活动。您也可以使用数据探索活动的输出来了解需要对数据执行的数据清洗活动的程度。

## 使用 Pandas 进行数据清洗
为了演示数据清洗步骤，我们将使用数据库中的 seat_type 表。

该表只有最小的数据量，因此在进行数据清洗之前我们将插入一些数据。

seat_type 中的数据如下图所示。它有三个列：座位类型名称、该座位类型的描述，以及一个提供该座位类型质量相对排名的列：

```
con=wr.mysql.connect("wranglerbook")
cursor=con.cursor()
cursor.execute("show columns from seat_type")
cursor.fetchall()
```

```
(('name', 'varchar(15)', 'NO', 'PRI', None, ''),
('description', 'varchar(120)', 'YES', '', None, ''),
('relative_quality', 'tinyint', 'YES', '', None, ''))
```

```
cursor.execute("select * from seat_type ;")
cursor.fetchall()
```

```
(('luxury', 'Excellent seats – box seats, behind the plate, etc. etc.', 1),
('obstructed', 'Pretty crappy, partially obstructed seats', 5),
('premium', 'Really good seats – first level, mid field, etc. etc.', 2),
('standard', 'Standard seats – not super awesome but pretty good', 3),
('standing', "Really? That's not a seat at all now, is it?", 10),
('sub-standard', 'End zone, nose bleed, etc.', 4))
```

图 9.38：探索座位表中当前可用的数据

我们将修改数据以模拟一个数据集，帮助我们逐步进行数据清洗活动。与其删除或更新座位表中的当前数据，不如插入新记录，这些记录是当前数据的模拟版本。

以下截图显示了 seat_type 表中具有相同列的新模拟记录。然而，description 列现在具有额外详细信息，例如座位类型可用的体育场层级以及该特定类型可用的座位数，以及描述。

```
cursor.execute("insert into seat_type values('Luxury1','level5,1500 seats,Comfortable seats,Leather seats!!!',1);")
cursor.execute("insert into seat_type values('Obstructed1','level3,100 seats,Comfortable seats though,But wont have any v...")
cursor.execute("insert into seat_type values('Premium1','level4-5,500 seats,Comfortable but normal seats, good views for ...")
cursor.execute("insert into seat_type values('Standard1','level1,5500 seats,Comfortable seats,Leather seats!!!',3);")
cursor.execute("insert into seat_type values('standing1','Level1-5,Comfortable seats,Leather seats!!!',10);")
cursor.execute("insert into seat_type values('sub-standard1','level1,200 seats,Front row , risk of injury!!!',4);")
```

图 9.39：座位表的模拟记录

对于当前用例，我们将仅提取插入表中的数据。我们将筛选座位类型名称列的值以字母（a-z）开头并以数字值（0-9）结尾的记录，这将仅获取名称列末尾附加数字 1 的新插入记录。

```
#Filter data that starts with characters and ends with a single number
cleanse_data=output_data[output_data["description"].str.contains("^[a-zA-Z]*[0-9]+")]
cleanse_data
```

|  | name | description | relative_quality |
|---|---|---|---|
| 1 | Luxury1 | level5,1500 seats,Comfortable seats,Leather se... | 1 |
| 3 | Obstructed1 | level3,100 seats,Comfortable seats though,But ... | 5 |
| 5 | Premium1 | level4-5,500 seats,Comfortable but normal seat... | 2 |
| 7 | Standard1 | level1,5500 seats,Comfortable seats,Leather se... | 3 |
| 9 | standing1 | Level1-5,Comfortable seats,Leather seats!!! | 10 |
| 11 | sub-standard1 | level1,200 seats,Front row , risk of injury!!! | 4 |

图 9.40：单独提取模拟数据记录用于数据清洗活动

然后我们将拆分 description 列中的数据，以便对数据进行更好的分析。我们将在逗号分隔符上使用 split 函数，并将数据获取到不同的列中，如以下截图所示：

```
#Extract data from column into different columns
pd.options.mode.chained_assignment = None
cleanse_data["Level_data"]=cleanse_data["description"].str.split(",",n=2).str[0]
cleanse_data["No_of_seats"]=cleanse_data["description"].str.split(",",n=2).str[1]
cleanse_data["Seat description"]=cleanse_data["description"].str.split(",",n=2).str[2]
```

识别异常值 删除空值并填入有效值

|  | name | description | relative_quality | Level_data | No_of_seats | Seat description |
|---|---|---|---|---|---|---|
| 1 | Luxury1 | level5,1500 seats,Comfortable seats,Leather se... | 1 | level5 | 1500 seats | Comfortable seats,Leather seats!!! |
| 3 | Obstructed1 | level3,100 seats,Comfortable seats though,But ... | 5 | level3 | 100 seats | Comfortable seats though,But wont have any vis... |
| 5 | Premium1 | level4-5,500 seats,Comfortable but normal seat... | 2 | level4-5 | 500 seats | Comfortable but normal seats, good views for t... |
| 7 | Standard1 | level1,5500 seats,Comfortable seats,Leather se... | 3 | level1 | 5500 seats | Comfortable seats,Leather seats!!! |
| 9 | standing1 | Level1-5,Comfortable seats,Leather seats!!! | 10 | Level1-5 | Comfortable seats | Leather seats!!! |
| 11 | sub-standard1 | level1,200 seats,Front row , risk of injury!!! | 4 | level1 | 200 seats | Front row , risk of injury!!! |

图 9.41：验证更新后的 flatten 函数的工作原理

现在我们已将数据拆分为多个列，但数据仍然不够干净，无法进行最终分析。让我们一次修复一个列的问题，以便更好地理解。Level_data 列提供了有关体育场的哪个层级有该类型座位可用的信息。然而，
