
## 从 JSON 文本中提取值

如本章前面所述，JSON 具有四种基本类型（字符串、数字、布尔值和 null）和两种复杂（结构）类型：对象和数组。SQL Server 2016 提供了两个函数来从 JSON 文本中提取值：

-   `JSON_VALUE`：用于提取基本数据类型的值。
-   `JSON_QUERY`：用于提取 JSON 片段或获取复杂值（对象或数组）。

### JSON_VALUE

`JSON_VALUE` 函数从 JSON 字符串中提取标量值。它接受两个输入参数：

-   **表达式：** 这是 Unicode 格式的 JSON 文本。
-   **路径：** 这是一个可选参数；它是一个 JSON 路径表达式，可用于指定输入表达式中的某个片段。

该函数的返回类型为 `nvarchar(4000)`，与输入表达式具有相同的排序规则。如果提取的值超过 4,000 个字符，则在路径为宽松模式（lax mode）时函数返回 NULL，在严格模式（strict mode）时返回错误消息。

如果表达式或路径无效，`JSON_VALUE` 函数会返回错误，说明 JSON 文本格式不正确。

以下示例展示了 `JSON_VALUE` 函数的实际用法。它用于返回属性和数组元素的值：

```
DECLARE @json NVARCHAR(MAX) = N'{
"Album":"Wish You Were Here",
"Year":1975,
"IsVinyl":true,
"Members":["Gilmour","Waters","Wright","Mason"]
}';
SELECT
    JSON_VALUE(@json, '$.Album') AS album,
    JSON_VALUE(@json, '$.Year') AS yr,
    JSON_VALUE(@json, '$.IsVinyl') AS isVinyl,
    JSON_VALUE(@json, '$.Members[0]') AS member1;
```

以下是上述查询的结果：

| album | yr | isVinyl | member1 |
|---|---|---|---|
| Wish You Were Here | 1975 | true | Gilmour |

请注意，所有返回的值都是字符串；如前所述，返回值的数据类型是 nvarchar。

该函数的目的是提取标量值。因此，如果 JSON 路径指定的是数组或对象，它将无法工作。以下使用前一个示例中 JSON 字符串的调用将返回 NULL 值：

```
DECLARE @json NVARCHAR(MAX) = N'{
"Album":"Wish You Were Here",
"Year":1975,
"IsVinyl":true,
"Members":["Gilmour","Waters","Wright","Mason"]
}';
SELECT
    JSON_VALUE(@json, '$.Members') AS member;
```

JSON 路径 $.members 指定了一个数组，而该函数期望的是标量值。即使路径表达式指定的属性不存在，也会返回 NULL 值。如前所述，JSON 路径表达式有两种模式：宽松模式和严格模式。在默认的宽松模式下，错误会被抑制，函数返回 NULL 值或空表；而在严格模式下，任何意外或不存在的路径都会引发批处理级异常。如果使用严格模式的 JSON 路径执行同样的调用，则最终会出错：

```
SELECT
    JSON_VALUE(@json, 'strict $.Members') AS member;
```

以下是错误消息：

```
Msg 13623, Level 16, State 1, Line 75
Scalar value cannot be found in the specified JSON path.
```

如果 JSON 属性值或字符串元素的长度超过 4,000，函数将返回 NULL。下一个示例通过使用两个超长字符串作为两个属性的值来演示这一点。第一个有 4,000 个字符，第二个多一个字符：

```
DECLARE @json NVARCHAR(MAX) = CONCAT('{"name":"', REPLICATE('A',4000), '"}');
DECLARE @json4001 NVARCHAR(MAX) = CONCAT('{"name":"', REPLICATE('A',4001), '"}')
SELECT
    JSON_VALUE(@json, '$.name') AS name4000,
    JSON_VALUE(@json4001, '$.name') AS name4001;
```

简化结果如下：

```
Name4000              name4001
--------------------   ---------
AAAAAAAAAAAAAAAA...   NULL
```

可以看到，4,001 对 JSON_VALUE 来说太长了，函数返回 NULL。如果在前一个示例中指定 strict，函数将返回错误：

```
DECLARE @json4001 NVARCHAR(MAX) = CONCAT('{"name":"', REPLICATE('A',4001), '"}')
SELECT
    JSON_VALUE(@json4001, ' strict $.name') AS name4001;
```

以下是错误消息：

```
Msg 13625, Level 16, State 1, Line 65
String value in the specified JSON path would be truncated.
```

这是关于 JSON 路径模式的函数行为的典型变化。宽松模式通常会返回 NULL 并且不会中断代码，而严格模式会引发批处理级异常。

JSON_VALUE 可以在 SELECT、WHERE 和 ORDER BY 子句中使用。在以下示例中，它被用在了这三个子句中：

```
SELECT
    PersonID,
    JSON_VALUE(UserPreferences, '$.timeZone') AS TimeZone,
    JSON_VALUE(UserPreferences, '$.table.pageLength') AS PageLength
FROM Application.People
WHERE JSON_VALUE(UserPreferences, '$.dateFormat') = 'yy-mm-dd'
    AND JSON_VALUE(UserPreferences, '$.theme') = 'blitzer'
ORDER BY JSON_VALUE(UserPreferences, '$.theme'), PersonID;
```

SQL Server 2016 中 JSON_VALUE 函数的一个重要限制是不允许将变量作为第二个参数（JSON 路径）。例如，以下代码在 SQL Server 2016 中将无法工作：

```
DECLARE @jsonPath NVARCHAR(10) = N'$.Album';
DECLARE @json NVARCHAR(200) = N'{
"Album":"Wish You Were Here",
"Year":1975
}';
SELECT
    JSON_VALUE(@json, @jsonPath) AS album;
```

该查询会失败，并出现以下错误消息：

```
Msg 13610, Level 16, State 1, Line 137
The argument 2 of the "JSON_VALUE or JSON_QUERY" must be a string literal
```

这是一个重大的限制；你必须预先提供静态的 JSON 路径，并且无法动态添加或更改它。幸运的是，此限制已在 SQL Server 2017 中移除，前面的代码在 SQL Server 2017 中会提供以下结果：

```
album
--------------------
Wish You Were Here
```

![](img/07d1225e179d5df9f2b2d26b026386c6_113_0.png)

在 SQL Server 2017 中，即使数据库仍处于兼容模式 130（对应 SQL Server 2016），你也可以为 JSON_VALUE 函数的两个参数使用变量。

SQL Server 2016 和 2017 中的 JSON 实现之间没有太多差异；这是最重要的一项。

### JSON_QUERY

JSON_QUERY 函数根据指定的 JSON 路径从输入的 JSON 字符串中提取 JSON 片段。它返回一个 JSON 对象或数组，因此其输出符合 JSON 格式。该函数与 JSON_VALUE 函数互补。

JSON_QUERY 始终返回符合 JSON 的文本。因此，如果你想向 SQL Server 表明该字符串是 JSON 格式化的，就应该使用此函数将其包裹起来。

该函数有两个输入参数：

-   **表达式：** 这是包含 JSON 文本的变量或列。
-   **路径：** 这是指定要提取的对象或数组的 JSON 路径。此参数是可选的。如果未指定，将返回整个输入字符串。

如果输入字符串定义为 (n)varchar(max)，则函数的返回类型为 nvarchar(max)；否则为 nvarchar(4000)。如前所述，该函数始终返回符合 JSON 的字符串。

如果表达式或路径无效，JSON_QUERY 会返回错误消息，指出 JSON 文本或 JSON 路径格式不正确。

在以下不言自明的示例中，演示了如何将此函数用于不同的 JSON 路径表达式：

```
DECLARE @json NVARCHAR(MAX) = N'{
"Album":"Wish You Were Here",
"Year":1975,
"IsVinyl":true,
"Songs":[{"Title":"Shine On You Crazy Diamond","Writers":"Gilmour, Waters, Wright"},
{"Title":"Have a Cigar","Writers":"Waters"},
{"Title":"Welcome to the Machine","Writers":"Waters"},
{"Title":"Wish You Were Here","Writers":"Gilmour, Waters"}],
"Members":{"Guitar":"David Gilmour","Bass Guitar":"Roger Waters","Keyboard":"Richard Wright","Drums":"Nick Mason"}
}';
--get Songs JSON fragment (array)
SELECT JSON_QUERY(@json,'$.Songs');
--get Members JSON fragment (object)
SELECT JSON_QUERY(@json,'$.Members');
--get fourth Song JSON fragment (object)
SELECT JSON_QUERY(@json,'$.Songs[3]');
```

以下是这些调用的结果：

```
[{"Title":"Shine On You Crazy Diamond","Writers":"Gilmour, Waters, Wright"},
{"Title":"Have a Cigar","Writers":"Waters"},
{"Title":"Welcome to the Machine","Writers":"Waters"},
{"Title":"Wish You Were Here","Writers":"Gilmour, Waters"}]
```

