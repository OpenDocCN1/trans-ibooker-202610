
{"Guitar":"David Gilmour","Bass Guitar":"Roger Waters","Keyboard":"Richard Wright","Drums":"Nick Mason"}
{"Title":"Wish You Were Here","Writers":"Gilmour, Waters"}

您可以看到返回的值是JSON对象和数组。但是，如果您指定的值不是数组或对象，该函数在宽松模式下返回NULL，在严格模式下返回错误：

```
--get property value (number)
SELECT JSON_QUERY(@json,'$.Year');
--get property value (string)
SELECT JSON_QUERY(@json,'$.Songs[1].Title');
--get value for non-existing property
SELECT JSON_QUERY(@json,'$.Studios');
```

这三个调用都返回NULL，而严格模式会引发批处理级异常：

```
SELECT JSON_QUERY(@json,'strict $.Year');
/*Result:
Msg 13624, Level 16, State 1, Line 54
Object or array cannot be found in the specified JSON path.
*/
--get value for non-existing property
SELECT JSON_QUERY(@json,'strict $.Studios');
/*Result:
Msg 13608, Level 16, State 5, Line 60
Property cannot be found on the specified JSON path
*/
```

您还可以使用JSON_QUERY来确保表列中JSON数据的完整性。例如，以下检查约束确保People表中的所有人在CustomFields列有值时，该列中的OtherLanguages属性也存在：

```
USE WideWorldImporters;
ALTER TABLE Application.People
ADD CONSTRAINT CHK_OtherLanguagesRequired
CHECK (JSON_QUERY(CustomFields, '$.OtherLanguages') IS NOT NULL OR
CustomFields IS NULL);
```

JSON_QUERY函数在SQL Server 2016中对于路径参数的限制与JSON_VALUE相同；只允许使用字面量。在SQL Server 2017中，您也可以使用变量。

## 修改JSON数据

有时您可能需要仅更新JSON数据的一部分。在SQL Server 2016中，您可以使用JSON_MODIFY函数修改JSON数据。它允许您：

- 更新现有属性的值
- 向现有数组添加新元素
- 插入新属性及其值
- 根据模式和所提供值的组合删除属性

该函数接受三个必需输入参数：

- **Expression**：这是包含JSON文本的变量或列名。
- **Path**：这是JSON路径表达式，带有可选的append修饰符。
- **new_value**：这是路径表达式中所指定属性的新值。

JSON_MODIFY函数返回更新后的JSON字符串。在接下来的小节中，您将看到此函数的实际应用。

### 添加新的JSON属性

在以下代码示例中，您添加一个名为IsVinyl的新属性，其值为true：

```
DECLARE @json NVARCHAR(MAX) = N'{
"Album":"Wish You Were Here",
"Year":1975
}';
PRINT JSON_MODIFY(@json, '$.IsVinyl', CAST(1 AS BIT));
```

您需要将值显式转换为BIT数据类型；否则，它将被双引号包围并被解释为字符串。以下是修改的结果：

```
{
"Album":"Wish You Were Here",
"Year":1975,
"IsVinyl":true
}
```

请注意，JSON路径表达式处于默认的宽松模式。如果指定严格模式，该函数将返回错误：

```
DECLARE @json NVARCHAR(MAX) = N'{
"Album":"Wish You Were Here",
"Year":1975
}';
PRINT JSON_MODIFY(@json, 'strict $.IsVinyl', CAST(1 AS BIT));
```

严格模式始终期望JSON路径表达式指定的属性存在。如果不存在，则返回以下错误消息：

```
Msg 13608, Level 16, State 2, Line 34
Property cannot be found on the specified JSON path.
```

请注意，当您添加的值已经是JSON格式时。在下一个示例中，假设您想要添加一个名为Members的新属性，并且已经准备好了整个JSON数组：

```
DECLARE @json NVARCHAR(MAX) = N'{
"Album":"Wish You Were Here",
"Year":1975,
"IsVinyl":true
}';
DECLARE @members NVARCHAR(500) = N'["Gilmour","Waters","Wright","Mason"]';
PRINT JSON_MODIFY(@json, '$.Members', @members);
```

一个新的Members属性已添加到输入的JSON数据中，但我们的JSON格式值被解释为文本，因此所有特殊字符都被转义。以下是修改后的输入字符串：

```
{
"Album":"Wish You Were Here",
"Year":1975,
"IsVinyl":true,
"Members":"[\"Gilmour\",\"Waters\",\"Wright\",\"Mason\"]"
}
```

为了避免对JSON格式文本进行转义，您需要告诉函数该文本已经是JSON，不应执行转义。您可以通过使用JSON_QUERY函数包装新值来实现这一点：

```
DECLARE @json NVARCHAR(MAX) = N'{
"Album":"Wish You Were Here",
"Year":1975,
"IsVinyl":true
}';
```

您可以看到，如果路径指定的属性存在，宽松模式和严格模式之间没有区别。

以下示例演示如何更新JSON文本中数组元素的值。假设您想将Members数组的第一个元素（Gilmour）替换为值（Barrett）：

```
DECLARE @json NVARCHAR(MAX) = N'{
"Album":"Wish You Were Here",
"Year":1975,
"Members":["Gilmour","Waters","Wright","Mason"]
}';
PRINT JSON_MODIFY(@json, '$.Members[0]', 'Barrett');
```

以下是预期结果：

```
{
"Album":"Wish You Were Here",
"Year":1975,
"Members":["Barrett","Waters","Wright","Mason"]
}
```

如果想向数组添加新元素，必须使用append。在以下示例中，您只需在Members数组中添加另一个元素：

```
DECLARE @json NVARCHAR(MAX) = N'{
"Album":"Wish You Were Here",
"Year":1975,
"Members":["Gilmour","Waters","Wright","Mason"]
}';
PRINT JSON_MODIFY(@json, 'append $.Members', 'Barrett');
```

结果如下：

```
{
"Album":"Wish You Were Here",
"Year":1975,
"Members":["Gilmour","Waters","Wright","Mason","Barrett"]
}
```

如果指定的索引超出范围或数组不存在，您将得到：

- **严格模式：** 这会显示错误消息且不返回任何值（批处理级异常）。
- **宽松模式：** 这不会显示错误；返回原始输入字符串。

要将JSON属性的值更新为NULL，必须在严格模式下使用JSON路径。使用以下代码将输入JSON字符串中的Year属性更新为NULL值：

```
DECLARE @json NVARCHAR(MAX) = N'{
"Album":"Wish You Were Here",
"Year":1975,
"Members":["Gilmour","Waters","Wright","Mason"]
}';
PRINT JSON_MODIFY(@json, 'strict $.Year', NULL);
```

输出如下。

```
{
"Album":"Wish You Were Here",
"Year":null,
"Members":["Gilmour","Waters","Wright","Mason"]
}
```

### 删除JSON属性

要从输入JSON字符串中删除属性，必须在宽松模式下使用JSON路径表达式。您将重复前面的代码，但这次使用宽松模式：

```
DECLARE @json NVARCHAR(MAX) = N'{
"Album":"Wish You Were Here",
"Year":1975,
"Members":["Gilmour","Waters","Wright","Mason"]
}';
PRINT JSON_MODIFY(@json, '$.Year', NULL);
```

当您观察此操作的结果时，可以看到Year属性不再存在：

```
{
"Album":"Wish You Were Here",
"Members":["Gilmour","Waters","Wright","Mason"]
}
```

通过这种方法，您只能删除属性及其值。您不能删除数组元素。以下代码不会从JSON数组属性Members中删除Waters元素；它实际上会将其更新为NULL：

```
DECLARE @json NVARCHAR(MAX) = N'{
"Album":"Wish You Were Here",
"Year":1975,
```

该计划显示执行了聚集索引扫描；SQL Server无法以高效方式在JSON列中搜索全名。

为了提高查询性能，您可以使用与WHERE子句中相同的表达式创建计算列，然后在其上使用非聚集索引：

```
ALTER TABLE dbo.T1 ADD FullName AS JSON_VALUE(info, '$.FullName');
CREATE INDEX IX1 ON dbo.T1(FullName);
```

当您再次执行相同的查询时，执行计划发生了变化：

```
SELECT id, info
FROM dbo.T1
WHERE JSON_VALUE(info,'$.FullName') = 'Vilma Niva';
```

新创建的索引被使用，该计划更加高效，如下图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_124_0.png)

使用计算列上索引的执行计划

当然，这只适用于特定的JSON路径，在这种情况下仅适用于FullName属性。对于其他属性，您需要在其上创建额外的计算列和索引。对于XML索引，所有节点和值都被覆盖；它们与特定值无关。

JSON索引的一个重要特性是它们感知排序规则。JSON_VALUE函数的结果是一个文本值，它从输入表达式继承其排序规则。因此，索引中的值使用源列中定义的排序规则进行排序。

通过在计算列上使用索引，您可以提高常用查询的性能。

