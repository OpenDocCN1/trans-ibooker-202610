

## Script 命令

script 命令基于一个或多个迁移创建 SQL 脚本。该命令有两个可选参数，分别表示起始迁移和结束迁移。如果两者均未指定，则会为所有迁移生成脚本。表 22-13 描述了这些参数。

### 表 22-13. EF Core 迁移 Script 命令的参数

| 参数 | 含义 |
|---|---|
| `<FROM>` | 起始迁移。默认为 0（零），即初始迁移。 |
| `<TO>` | 目标迁移。默认为最后一个迁移。 |

如果未指定迁移名称，生成的脚本将包含所有迁移的累计内容。如果指定了迁移名称，脚本将包含这两个迁移之间（含两者）的更改。每个迁移都封装在一个事务中。如果执行脚本的数据库中不存在 `__EFMigrationsHistory` 表，则会创建该表。该表也会更新，以反映已执行的迁移。以下是一些示例：

```
// 为所有迁移生成脚本
dotnet ef migrations script

// 从初始迁移开始，生成到 Many2Many 迁移为止的脚本
dotnet ef migrations script 0 Many2Many
```

还有一些其他选项，如表 22-14 所示。`-o` 选项允许指定脚本文件（目录路径相对于命令的执行位置），`-i` 则会创建一个幂等脚本。这意味着脚本会检查某个迁移是否已应用；如果已应用，则跳过该迁移。`--no-transaction` 选项会禁用通常添加到脚本中的事务。

### 表 22-14. EF Core 迁移 Script 命令的其他选项

| 选项（简写 \|\| 全称） | 含义 |
|---|---|
| `-o` \|\| `--output <FILE>` | 写入生成脚本的文件 |
| `-i` \|\| `--idempotent` | 生成脚本，在应用迁移前检查该迁移是否已应用 |
| `--no-transactions` | 不将每个迁移封装在事务中 |

## 数据库命令

数据库命令有两个：drop 和 update。drop 命令会删除数据库（如果数据库存在）。update 命令使用迁移更新数据库。

## Drop 命令

drop 命令会删除 `DbContext` 的 `OnConfiguring` 方法中上下文工厂所指定连接字符串对应的数据库。使用 force 选项时，命令不会请求确认，并会强制关闭所有连接。参见表 22-15。

### 表 22-15. EF Core 数据库 Drop 选项

| 选项（简写 \|\| 全称） | 含义 |
|---|---|
| `-f` \|\| `--force` | 不询问确认，强制关闭所有连接。 |
| `--dry-run` | 显示将要删除的数据库，但不实际删除。 |

## Database Update 命令

update 命令接受一个参数（迁移名称）以及常用选项。该命令还有一个额外选项：`--connection <CONNECTION>`。它允许使用未在设计时工厂或 `DbContext` 中配置的连接字符串。

如果执行命令时未指定迁移名称，命令会将数据库更新到最新迁移；必要时还会创建数据库。如果指定了迁移名称，数据库将更新到该迁移。所有尚未应用的先前迁移也会一并应用。应用迁移时，其名称会存储在 `__EFMigrationsHistory` 表中。

如果指定迁移的时间戳早于其他已应用的迁移，则所有时间戳更晚的迁移都会回滚。如果指定迁移为 0（零），则会还原所有迁移，数据库将变为空数据库（但 `__EFMigrationsHistory` 表仍会保留）。

## DbContext 命令

DbContext 命令共有四个。其中三个（list、info、script）用于项目中的派生 `DbContext` 类。scaffold 命令则根据现有数据库创建派生 `DbContext` 和实体。表 22-16 列出了这四个命令。

### 表 22-16. DbContext 命令

| 命令 | 含义 |
|---|---|
| Info | 获取有关 `DbContext` 类型的信息 |
| List | 列出可用的 `DbContext` 类型 |
| Scaffold | 为数据库生成 `DbContext` 和实体类型 |
| Script | 根据对象模型从 `DbContext` 生成 SQL 脚本，不使用任何迁移 |

list 和 info 命令支持常用选项。list 命令会列出目标项目中的派生 `DbContext` 类。info 命令会提供指定派生 `DbContext` 类的详细信息，包括连接字符串、提供程序名称、数据库名称和数据源。script 命令会根据对象模型创建 SQL 脚本来创建数据库，并忽略可能存在的任何迁移。scaffold 命令用于对现有数据库进行反向工程，下一节将对此进行介绍。

## DbContext Scaffold 命令

scaffold 命令根据现有数据库创建 C# 类（派生 `DbContext` 和实体），并可按需包含数据注释和 Fluent API 命令。该命令有两个必需参数：数据库连接字符串和提供程序的完全限定名称（例如 `Microsoft.EntityFrameworkCore.SqlServer`）。表 22-17 描述了这些参数。

### 表 22-17. DbContext Scaffold 参数

| 参数 | 含义 |
|---|---|
| Connection | 数据库连接字符串 |
| Provider | 要使用的 EF Core 数据库提供程序（例如 `Microsoft.EntityFrameworkCore.SqlServer`） |

可用选项包括：选择特定架构和表、指定所创建上下文类的名称和命名空间、指定生成的实体类的输出目录和命名空间，等等。此外，还可以使用标准选项。扩展选项列于表 22-18，后文将对此进行讨论。

### 表 22-18. DbContext Scaffold 选项

| 选项（简写 \|\| 全称） | 含义 |
|---|---|
| `-d` \|\| `--data-annotations` | 尽可能使用特性配置模型。如果省略，则只使用 Fluent API。 |
| `-c` \|\| `--context <NAME>` | 要创建的派生 `DbContext` 的名称。 |
| `--context-dir <PATH>` | 放置派生 `DbContext` 的目录，相对于项目目录。默认为数据库名称。 |
| `-f` \|\| `--force` | 替换目标目录中的任何现有文件。 |
| `-o` \|\| `--output-dir <PATH>` | 放置生成的实体类的目录，相对于项目目录。 |
| `--schema <SCHEMA_NAME>...` | 要为其生成实体类型的表所在的架构。 |
| `-t` \|\| `--table <TABLE_NAME>...` | 要为其生成实体类型的表。 |
| `--use-database-names` | 直接使用数据库中的表名和列名。 |
| `-n` \|\| `--namespaces <NAMESPACE>` | 生成的实体类的命名空间。默认与目录匹配。 |
| `--context-namespace <NAMESPACE>` | 生成的派生 `DbContext` 类的命名空间。默认与目录匹配。 |
| `--no-onconfiguring` | 不生成 `OnConfiguring` 方法。 |
| `--no-pluralize` | 不使用复数化器。 |

在 EF Core 5.0 中，scaffold 命令变得更加强大。如你所见，可选项非常丰富。如果选择数据注释（`-d`）选项，EF Core 会在可行的情况下使用数据注释，并通过 Fluent API 补充两者之间的差异。如果不选择该选项，则整个配置（与约定不同时）都会通过 Fluent API 编写。你可以指定生成的实体和派生 `DbContext` 文件的命名空间、架构和位置。如果不想为整个数据库生成代码，可以选择特定的架构和表。`--no-onconfiguring` 选项会从生成的类中移除 `OnConfiguring()` 方法；`--no-pluralize` 选项则会关闭复数化器。复数化器在创建迁移时会将单数实体（Car）转换为复数表（Cars），在生成实体时则会将复数表转换为单数实体。

## 总结

本章开启了对 Entity Framework Core 的探索。本章介绍了 EF Core 的基础知识、查询的执行方式以及更改跟踪。你了解了如何塑造模型、EF Core 约定、数据注释和 Fluent API，以及它们如何影响数据库设计。最后一节介绍了 EF Core 命令行界面和全局工具的强大功能。

本章涵盖了大量理论知识和一些代码，而下一章几乎全是代码，只包含少量理论内容。完成第 23 章后，你将拥有完整的 AutoLot 数据访问层。

