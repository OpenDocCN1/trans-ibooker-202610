
https://github.com/markjprice/cs10dotnet6/blob/main/book-links.md#chapter-10---working-with-data-using-entity-framework-core

## 练习 10.4 – 探索 NoSQL 数据库

本章重点介绍了 SQL Server 和 SQLite 等关系型数据库管理系统（RDBMS）。如果您想了解更多关于 NoSQL 数据库（如 Cosmos DB 和 MongoDB）以及如何将其与 EF Core 一起使用，那么我推荐以下链接：

- **欢迎使用 Azure Cosmos DB**: https://docs.microsoft.com/en-us/azure/cosmos-db/introduction
- **将 NoSQL 数据库用作持久化基础设施**: https://docs.microsoft.com/en-us/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/nosql-database-persistence-infrastructure
- **Entity Framework Core 的文档数据库提供程序**: https://github.com/BlueshiftSoftware/EntityFrameworkCore

## 摘要

本章中，您学习了如何连接到现有数据库、如何执行简单的 LINQ 查询并处理结果、如何使用筛选的 Include、如何添加、修改和删除数据，以及如何为现有数据库（如 Northwind）构建实体数据模型。您还学习了如何定义 Code First 模型并使用它创建新数据库并填充数据。

在下一章中，您将学习如何编写更高级的 LINQ 查询以进行选择、筛选、排序、连接和分组。

## 11

### 使用 LINQ 查询和操作数据

本章介绍 **语言集成查询 (LINQ)** 表达式。LINQ 是一组语言扩展，增加了处理项目序列然后对其进行筛选、排序并投影到不同输出的能力。

本章将涵盖以下主题：

- 编写 LINQ 表达式
- 使用 LINQ 操作集合
- 使用 LINQ 与 EF Core
- 使用语法糖美化 LINQ 语法
- 使用并行 LINQ 使用多线程
- 创建自己的 LINQ 扩展方法
- 使用 LINQ to XML

### 编写 LINQ 表达式

尽管我们在 *第 10 章，使用 Entity Framework Core 操作数据* 中编写了一些 LINQ 表达式，但它们并不是重点，因此我没有正确解释 LINQ 的工作原理，现在让我们花时间正确理解它们。

### LINQ 的构成

LINQ 有多个部分；有些是必需的，有些是可选的：

- **扩展方法（必需）**：包括示例 such as `Where`、`OrderBy` 和 `Select`。它们提供了 LINQ 的功能。
- **LINQ 提供程序（必需）**：包括用于处理内存中对象的 LINQ to Objects、用于处理存储在外部数据库中并使用 EF Core 建模的数据的 LINQ to Entities，以及用于处理存储为 XML 的数据的 LINQ to XML。这些提供程序以特定于不同数据类型的方式执行 LINQ 表达式。
- **Lambda 表达式（可选）**：可替代命名方法以简化 LINQ 查询，例如用于 where 方法筛选的条件逻辑。
- **LINQ 查询理解语法（可选）**：包括 from、in、where、orderby、descending 和 select 等 C# 关键字。它们是某些 LINQ 扩展方法的别名，使用它们可以简化您编写的查询，特别是如果您已有其他查询语言（如 **结构化查询语言（SQL）**）的经验。

当程序员首次接触 LINQ 时，他们经常认为 LINQ 查询理解语法就是 LINQ，但具有讽刺意味的是，这正是 LINQ 中可选的部分之一！

### 使用 Enumerable 类构建 LINQ 表达式

LINQ 扩展方法（如 where 和 Select）由 Enumerable 静态类附加到实现 IEnumerable<T> 的任何类型，即所谓的序列。

例如，任何类型的数组都实现 IEnumerable<T> 类，其中 T 是数组中项目的类型。这意味着所有数组都支持 LINQ 进行查询和操作。

所有泛型集合，如 List<T>、Dictionary<TKey, TValue>、Stack<T> 和 Queue<T>，都实现 IEnumerable<T>，因此也可以使用 LINQ 进行查询和操作。

Enumerable 定义了 50 多个扩展方法，如下表所示：

| Method(s) | Description |
|---|---|
| First, FirstOrDefault, Last, LastOrDefault | 获取序列中的第一个或最后一个项目，或抛出异常；如果没有第一个或最后一个项目，则返回该类型的默认值，例如 int 的 0 和引用类型的 null。 |
| Where | 返回匹配指定筛选器的项目序列。 |
| Single, SingleOrDefault | 返回匹配特定筛选器的项目或抛出异常；如果没有恰好一个匹配项，则返回该类型的默认值。 |
| ElementAt, ElementAtOrDefault | 返回指定索引位置的项目或抛出异常；如果该位置没有项目，则返回该类型的默认值。.NET 6 中新增了可传入 Index 而非 int 的重载，在处理 Span<T> 序列时更高效。 |
| Select, SelectMany | 将项目投影到不同的形状，即不同的类型，并展平嵌套的项目层次结构。 |
| OrderBy, OrderByDescending, ThenBy, ThenByDescending | 按指定的字段或属性对项目进行排序。 |
| Reverse | 反转项目的顺序。 |
| GroupBy, GroupJoin, Join | 对序列进行分组和/或连接两个序列。 |
| Skip, SkipWhile | 跳过一定数量的项目；或在表达式为 true 时跳过。 |
| Take, TakeWhile | 获取一定数量的项目；或在表达式为 true 时获取。.NET 6 中新增了可传入 Range 的 Take 重载，例如 Take(range: 3..^5) 表示获取从开头起第 3 个项目开始到从末尾起第 5 个项目结束的子集，或者代替 Skip(4) 可使用 Take(4..)。 |
| Aggregate, Average, Count, LongCount, Max, Min, Sum | 计算聚合值。 |
| TryGetNonEnumeratedCount | Count() 检查序列是否实现了 Count 属性并返回其值，否则枚举整个序列以计数其项目。.NET 6 中新增了此方法，它仅检查 Count，如果缺失则返回 false 并将 out 参数设为 0，以避免可能性能不佳的操作。 |
| All, Any, Contains | 如果所有或任意项目匹配筛选器，或序列包含指定项目，则返回 true。 |
| Cast | 将项目转换为指定类型。在编译器否则会抱怨的场景中，将非泛型对象转换为泛型类型时很有用。 |
| OfType | 移除不匹配指定类型的项目。 |
| Distinct | 移除重复项目。 |
| Except, Intersect, Union | 执行返回集合的操作。集合不能有重复项目。虽然输入可以是任何序列，因此输入可能有重复，但结果始终是集合。 |
| Chunk | 将序列划分为大小批次。 |
| Append, Concat, Prepend | 执行序列组合操作。 |
| Zip | 基于项目位置对两个序列执行匹配操作，例如第一个序列中位置 1 的项目与第二个序列中位置 1 的项目匹配。.NET 6 中新增了对三个序列的匹配操作。之前您需要两次运行两个序列的重载才能达到相同目标。 |
| ToArray, ToList, ToDictionary, ToHashSet, ToLookup | 将序列转换为数组或集合。这些是唯一执行 LINQ 表达式的扩展方法。 |
| DistinctBy, ExceptBy, IntersectBy, UnionBy, MinBy, MaxBy | .NET 6 中新增了 By 扩展方法。它们允许在项目的子集上进行比较而不是整个项目。例如，代替通过比较整个 Person 对象来移除重复项，您可以通过仅比较其 LastName 和 DateOfBirth 来移除重复项。 |

Enumerable 类还具有一些不是扩展方法的方法，如下表所示：

| Method | Description |
|---|---|
| Empty<T> | 返回指定类型 T 的空序列。它可用于将空序列传递给需要 IEnumerable<T> 的方法。 |
| Range | 返回从起始值开始包含 count 个项目的整数序列。例如，Enumerable.Range(start: 5, count: 3) 将包含整数 5、6 和 7。 |
| Repeat | 返回包含相同元素重复 count 次的序列。例如，Enumerable.Repeat(element: "5", count: 3) 将包含字符串值 "5"、"5" 和 "5"。 |

### 理解延迟执行

LINQ 使用延迟执行。重要的是要理解，调用这些扩展方法中的大多数并不会执行查询并获取结果。这些扩展方法中的大多数返回一个 LINQ 表达式，表示一个问题，而不是答案。让我们探索一下：

- 1. 使用您喜欢的代码编辑器创建一个名为 Chapter11 的新解决方案/工作区。
- 2. 添加控制台应用程序项目，如以下列表中定义：
  1. 项目模板：Console Application / console
  2. 工作区/解决方案文件和文件夹：Chapter11
  3. 项目文件和文件夹：LinqWithObjects
- 3. 在 Program.cs 中，删除现有代码并静态导入 Console。
- 4. 添加语句以定义在办公室工作的人员的字符串值序列，如以下代码所示：
- 5. 要提出问题并获得答案，即执行查询，您必须通过调用其中一个 "To" 方法（如 ToArray 或 ToLookup）或枚举查询来将其具体化，如以下代码所示：

// a string array is a sequence that implements IEnumerable<string>
string[] names = new[] { "Michael", "Pam", "Jim", "Dwight",
"Angela", "Kevin", "Toby", "Creed" };

WriteLine("Deferred execution");

// Question: Which names end with an M?
// (written using a LINQ extension method)
var query1 = names.Where(name => name.EndsWith("m"));

// Question: Which names end with an M?
// (written using LINQ query comprehension syntax)
var query2 = from name in names where name.EndsWith("m") select name;
