

## 混合客户端-服务器评估

EF Core 的早期版本引入了混合服务器端和客户端执行的功能。这意味着可以在 LINQ 语句中间使用 C# 函数，从而基本抵消我在上一段中所描述的内容。C# 函数之前的部分会在服务器端执行，但随后该时刻查询产生的所有结果都会被带回客户端，然后查询的其余部分会作为 LINQ to Objects 执行。这最终带来的问题多于它解决的问题，因此随着 EF Core 3.1 的发布，此功能发生了改变。现在，只有 LINQ 语句的最终节点可以在客户端执行。

## 跟踪查询与非跟踪查询

当数据从数据库读取到 `DbSet<T>` 实例中时，实体默认会由更改跟踪器进行跟踪。这通常正是应用程序所需要的。一旦某个实例被更改跟踪器跟踪，后续针对同一项数据（基于主键）向数据库发出的调用将导致更新该项，而不会创建重复项。

不过，有时你可能需要从数据库获取一些数据，但不希望这些数据被更改跟踪器跟踪。原因可能是性能问题（对于大量记录跟踪原始值和当前值可能会增加内存压力），或者你明确知道需要这些数据的应用程序部分永远不会修改这些记录。

若要将数据加载到 `DbSet<T>` 实例中，同时不将数据添加到 `ChangeTracker`，请在 LINQ 语句中加入 `AsNoTracking()`。这会通知 EF Core 获取数据，但不要将其添加到 `ChangeTracker` 中。例如，若要加载一条 Car 记录，同时不将其添加到 `ChangeTracker`，请执行以下代码：

```csharp
public virtual Car? FindAsNoTracking(int id)
    => Table.AsNoTracking().FirstOrDefault(x => x.Id == id);
```

这样做的好处是不会增加潜在的内存压力，但缺点是：后续获取同一辆 Car 的调用会创建该记录的额外副本。虽然需要使用更多内存，并且执行时间会略有增加，但可以修改查询，以确保未映射的 Car 只有一个实例。

```csharp
public virtual Car? FindAsNoTracking(int id)
    => Table.AsNoTrackingWithIdentityResolution().FirstOrDefault(x => x.Id == id);
```

## 值得注意的 EF Core 功能

EF Core 复制了 EF 6 中的许多功能，并且每个版本都会增加更多功能。其中许多功能在 EF Core 中都得到了大幅改进，无论是功能性还是性能方面。除了复制 EF 6 中的功能之外，EF Core 还包含许多上一版本中没有的新功能。以下是 EF Core 中一些较为重要的功能（不分先后顺序）。

> **注意：**本节中的代码示例直接取自已完成的 AutoLot 数据访问库，你将在下一章构建该库。

## 处理数据库生成的值

除了更改跟踪以及根据 LINQ 生成 SQL 查询之外，相较于直接使用 ADO.NET，使用 EF Core 的一个显著优势是可以无缝处理数据库生成的值。添加或更新实体后，EF Core 会查询数据库生成的数据，并自动使用正确的值更新实体。在原始 ADO.NET 中，则需要自行完成这些操作。

例如，Inventory 表有一个整数主键，该主键在 SQL Server 中定义为 Identity 列。添加记录时，Identity 列由 SQL Server 使用序列中的唯一编号填充；在正常更新期间不允许更新该列（启用 Identity insert 的特殊情况除外）。此外，Inventory 表还有一个用于并发检查的 Timestamp 列。下一节将介绍并发检查，但现在只需知道，Timestamp 列由 SQL Server 维护，并且会在添加或编辑操作时更新。

以向 Inventory 表添加一辆新的 Car 为例。以下代码创建一个新的 Car 实例，将其添加到派生 `DbContext` 上的 `DbSet<Car>` 实例中，然后调用 `SaveChanges()` 持久化数据：

```csharp
var car = new Car
{
    Color = "Yellow",
    MakeId = 1,
    PetName = "Herbie"
};
Context.Cars.Add(car);
Context.SaveChanges();
```

执行 `SaveChanges` 时，新记录会插入表中，然后 Id 和 Timestamp 值会从表返回到 EF Core，实体的相应属性也会随之更新。

```sql
INSERT INTO [dbo].[Inventory] ([Color], [MakeId], [PetName])
VALUES (N'Yellow', 1, N'Herbie');
SELECT [Id], [TimeStamp]
FROM [dbo].[Inventory]
WHERE @@ROWCOUNT = 1 AND [Id] = scope_identity();
```

> **注意：**EF Core 实际执行的是参数化查询，但为了便于阅读，我简化了所有示例。

向数据库添加多个项时同样有效。EF Core 知道如何将这些值关联到正确的实体。更新记录时，主键值已经已知，因此在我们的 Car 示例中，只会查询并返回更新后的 Timestamp 值。

## 并发检查

当两个独立的进程（用户或系统）大致同时尝试更新同一条记录时，就会出现并发问题。例如，用户 1 和用户 2 都获取了客户 A 的数据。用户 1 更新地址并保存更改。用户 2 更新信用评级，并尝试保存同一条记录。如果用户 2 的保存操作成功，由于地址是在用户 2 获取记录之后被更改的，用户 1 的更改将会被还原。另一种选择是让用户 2 的保存操作失败；这样，用户 1 的更改会被持久化，但用户 2 的更改不会被保存。

如何处理这种情况取决于应用程序的需求。解决方案范围从什么都不做（第二次更新覆盖第一次更新），到使用乐观并发（第二次更新失败），再到更复杂的解决方案，例如检查各个字段。除了选择什么都不做（这被普遍认为是不良的编程实践）之外，开发人员需要知道并发问题何时发生，以便采取适当的处理方式。

幸运的是，许多现代数据库都提供了帮助开发团队处理并发问题的工具。SQL Server 有一种名为 `timestamp` 的内置数据类型，它是 `rowversion` 的同义词。如果某列定义为 `timestamp` 数据类型，那么添加记录到数据库时，该列的值由 SQL Server 创建；更新记录时，该列的值也会随之更新。该值几乎可以保证是唯一的，并且由 SQL Server 控制。

EF Core 可以通过在实体上实现 `Timestamp` 属性来利用 SQL Server 的 `timestamp` 数据类型（在 C# 中表示为 `byte[]`）。使用 `Timestamp` 特性或 Fluent API 配置定义的实体属性，在更新或删除记录时会被添加到 `WHERE` 子句中。生成的 SQL 不再仅使用主键值，而是将 Timestamp 属性的值也添加到 `WHERE` 子句中。这样，结果就会被限制为主键值和 timestamp 值都匹配的记录。如果另一位用户（或系统）已经更新了该记录，timestamp 值就不会匹配，此时 `UPDATE` 或 `DELETE` 语句不会更新该记录。以下是一个使用 Timestamp 列的更新查询示例：

```sql
UPDATE [dbo].[Inventory] SET [Color] = N'Yellow'
WHERE [Id] = 1 AND [TimeStamp] = 0x000000000000081F;
```

当数据存储报告的受影响记录数与 `ChangeTracker` 预期更改的记录数不同时，EF Core 会抛出 `DbUpdateConcurrencyException`，并回滚整个事务。`DbUpdateConcurrencyException` 包含所有未能持久化的记录的信息，包括原始值（实体从数据库加载时的值）和当前值（用户或系统更新后的值）。此外，还有一种方法可以获取当前数据库值（这需要再次调用服务器）。借助这些丰富的信息，开发人员就可以根据应用程序的要求处理并发错误。以下代码展示了这一过程：

```csharp
try
{
    // 获取一条 Car 记录（哪一条都可以）
    var car = Context.Cars.First();
    // 在上下文之外更新数据库
    Context.Database.ExecuteSqlInterpolated($"Update dbo.Inventory set Color='Pink' where Id = {car.Id}");
    // 更新更改跟踪器中的 Car 记录，然后尝试保存更改
    car.Color = "Yellow";
    Context.SaveChanges();
}
catch (DbUpdateConcurrencyException ex)
{
    // 获取更新失败的实体
    var entry = ex.Entries[0];
    // 获取原始值（实体加载时的值）
    PropertyValues originalProps = entry.OriginalValues;
    // 获取当前值（由此代码路径更新后的值）
    PropertyValues currentProps = entry.CurrentValues;
    // 从数据存储中获取当前值
    // 注意：这需要再次调用数据库
    // PropertyValues databaseProps = entry.GetDatabaseValues();
}
```

