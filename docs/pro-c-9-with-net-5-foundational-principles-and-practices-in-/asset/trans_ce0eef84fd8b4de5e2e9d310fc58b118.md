

要“重置”数据库和项目，请删除 Migrations 文件夹和数据库。要使用 CLI 强制删除数据库，请输入以下命令：

```
dotnet ef database drop -f -c AutoLot.Samples.ApplicationDbContext
```

现在为 TPT 模式创建并应用迁移。

```
dotnet ef migrations add TPT -o Migrations -c AutoLot.Samples.ApplicationDbContext
dotnet ef database update TPT -c AutoLot.Samples.ApplicationDbContext
```

EF Core 在更新数据库时会创建以下表。索引也表明表之间存在一对一映射。

```
CREATE TABLE [dbo].[BaseEntities](
    [Id] [int] IDENTITY(1,1) NOT NULL,
    [TimeStamp] [varbinary](max) NULL,
    CONSTRAINT [PK_BaseEntities] PRIMARY KEY CLUSTERED
(
    [Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO
```

```
CREATE TABLE [dbo].[Inventory](
    [Id] [int] NOT NULL,
    [MakeId] [int] NOT NULL,
    [Color] [nvarchar](max) NULL,
    [PetName] [nvarchar](max) NULL,
    CONSTRAINT [PK_Inventory] PRIMARY KEY CLUSTERED
(
    [Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO
```

```
ALTER TABLE [dbo].[Inventory] WITH CHECK ADD  CONSTRAINT [FK_Inventory_BaseEntities_Id]
FOREIGN KEY([Id])
REFERENCES [dbo].[BaseEntities] ([Id])
GO
ALTER TABLE [dbo].[Inventory] CHECK CONSTRAINT [FK_Inventory_BaseEntities_Id]
GO
```

> **注意：** 表-per-类型映射具有显著的性能影响，在使用此映射方案之前应予以考虑。有关更多信息，请参阅文档：https://docs.microsoft.com/en-us/ef/core/performance/modeling-for-performance#inheritance-mapping。

要“重置”数据库和项目以准备下一组示例，请注释掉 `OnModelCreating()` 方法中的代码，并再次删除 Migrations 文件夹和数据库。

```
dotnet ef database drop -f -c AutoLot.Samples.ApplicationDbContext
```

## 导航属性和外键

*导航属性* 表示实体类之间的关系，并使代码能够从一个实体实例遍历到另一个实体实例。根据定义，导航属性是映射到数据库提供程序定义的非标量类型的任何属性。实际上，导航属性映射到另一个实体（称为 *引用导航属性*）或另一个实体的集合（称为 *集合导航属性*）。在数据库端，导航属性被转换为表之间的外键关系。EF Core 直接支持一对一、一对多以及（EF Core 5 中的新功能）多对多关系。实体类也可以具有指向自身的导航属性，表示自引用表。

> **注意：** 我认为将具有导航属性的对象视为链表是有帮助的，如果导航属性是双向的，则对象就像双向链表一样。

在介绍导航属性和实体关系模式的细节之前，请参阅表 22-4。这些术语在所有三种关系模式中都使用。

**表 22-4. 用于描述导航属性和关系的术语**

| 术语 | 生活中含义 |
|---|---|
| 主体实体 | 关系的父级。 |
| 从属实体 | 关系的子级。 |
| 主体键 | 用于定义主体实体的属性/属性。可以是主键或备用键。键可以使用单个属性或多个属性进行配置。 |
| 外键 | 子实体保存主体键的属性/属性。 |
| 必需关系 | 外键值是必需的（非空）关系。 |
| 可选关系 | 外键值不是必需的（可空）关系。 |

### 缺失的外键属性

如果具有引用导航属性的实体没有用于外键值的属性，EF Core 将在实体上创建必要的属性/属性。这些被称为 *影子外键属性*，其命名格式为 `<导航属性名称><主体键属性名称>` 或 `<主体实体名称><主体键属性名称>`。这适用于所有关系类型（一对多、一对一、多对多）。在实体中构建显式外键属性/属性比让 EF Core 为您创建要干净得多。

### 一对多关系

要创建一对多关系，位于一侧（主体）的实体类添加主体侧实体类的集合属性（从属）。从属实体还应具有指向主体的外键属性。如果没有，EF Core 将创建影子外键属性，如前所述。

例如，在第 21 章中创建的数据库中，Makes 表（由 Make 实体类表示）和 Inventory 表（由 Car 实体类表示）之间存在一对多关系。为了使这些示例保持简单，Car 实体将映射到 Cars 表。以下代码显示了表示此关系的双向导航属性：

```
using System.Collections.Generic;
namespace AutoLot.Samples.Models
{
    public class Make : BaseEntity
    {
        public string Name { get; set; }
        public IEnumerable<Car> Cars { get; set; } = new List<Car>();
    }
}
```

```
using System.Collections.Generic;
namespace AutoLot.Samples.Models
{
    public class Car : BaseEntity
    {
        public string Color { get; set; }
        public string PetName { get; set; }
        public int MakeId { get; set; }
        public Make MakeNavigation { get; set; }
    }
}
```

> **注意：** 在脚手架现有数据库时，EF Core 将引用导航属性命名为与属性类型名相同（例如，`public Make Make { get; set; }`）。这会导致导航和 IntelliSense 出现问题，更不用说使代码难以使用。我更喜欢为引用导航属性添加后缀 `Navigation` 以提高清晰度，如前例所示。

在 Car/Make 示例中，Car 实体是从属实体（一对多中的多），Make 实体是主体实体（一对多中的一）。

将 `DbSet<Make>` 实例添加到 `ApplicationDbContext`，如下所示：

```
public DbSet<Car> Cars { get; set; }
public DbSet<Make> Makes { get; set; }
```

使用以下命令创建迁移并更新数据库：

```
dotnet ef migrations add One2Many -o Migrations -c AutoLot.Samples.ApplicationDbContext
dotnet ef database update One2Many -c AutoLot.Samples.ApplicationDbContext
```

当使用 EF Core 迁移更新数据库时，会创建以下表：

```
CREATE TABLE [dbo].[Makes](
    [Id] [int] IDENTITY(1,1) NOT NULL,
    [Name] [nvarchar](max) NULL,
    [TimeStamp] [varbinary](max) NULL,
    CONSTRAINT [PK_Makes] PRIMARY KEY CLUSTERED
(
    [Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO
```

```
CREATE TABLE [dbo].[Cars](
    [Id] [int] IDENTITY(1,1) NOT NULL,
    [Color] [nvarchar](max) NULL,
    [PetName] [nvarchar](max) NULL,
    [TimeStamp] [varbinary](max) NULL,
    [MakeId] [int] NOT NULL,
    CONSTRAINT [PK_Cars] PRIMARY KEY CLUSTERED
(
    [Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO
```

```
ALTER TABLE [dbo].[Cars] WITH CHECK ADD  CONSTRAINT [FK_Cars_Makes_MakeId] FOREIGN KEY([MakeId])
REFERENCES [dbo].[Makes] ([Id])
ON DELETE CASCADE
GO
ALTER TABLE [dbo].[Cars] CHECK CONSTRAINT [FK_Cars_Makes_MakeId]
GO
```

请注意在从属表（Cars）上创建的外键和检查约束。

### 一对一关系

在一对一关系中，两个实体都具有指向另一个实体的引用导航属性。虽然一对多关系清楚地指出了主体和从属实体，但在构建一对一关系时，EF Core 必须被告知哪一侧是主体，要么是通过具有指向主体实体的明确定义的外键，要么是通过使用 Fluent API 指示主体。如果 EF Core 未被告知，它将根据其检测外键的能力进行选择。实际上，您应该通过添加外键属性来明确定义从属方。

```
namespace AutoLot.Samples.Models
{
    public class Car : BaseEntity
    {
```

