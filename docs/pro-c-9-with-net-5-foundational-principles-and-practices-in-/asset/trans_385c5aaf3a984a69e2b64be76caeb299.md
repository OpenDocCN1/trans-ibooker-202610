

## 连接到任意其他 SQL Server 实例

如果您要连接到其他 SQL Server 实例，请相应地更新连接属性。

## 恢复 AutoLot 数据库备份

与其从头构建数据库，您可以使用 SSMS 或 Azure Data Studio 恢复存储库中本章文件里提供的备份之一。提供了两个备份：名为 AutoLotWindows.ba_ 的备份专为 Windows 机器（LocalDb、Windows Server 等）使用而设计，而名为 AutoLotDocker.ba_ 的备份专为 Docker 容器中使用而设计。

> 注意 Git 默认忽略扩展名为 bak 的文件。在恢复数据库之前，您需要将扩展名从 ba_ 重命名为 bak。

## 将备份文件复制到您的容器

如果您在 Docker 容器中使用 SQL Server，则首先必须将备份文件复制到容器中。幸运的是，Docker CLI 提供了处理容器文件系统的机制。首先，在主机机器上的命令窗口中使用以下命令为备份创建一个新目录：

```
docker exec -it AutoLot mkdir var/opt/mssql/backup
```

路径结构必须与容器的操作系统（本例中为 Ubuntu）匹配，即使您的主机机器基于 Windows。接下来，使用以下命令将备份复制到新目录（将 AutoLotDocker.bak 的位置更新为本地机器的相对或绝对路径）：

```
[Windows]
docker cp .\AutoLotDocker.bak AutoLot:var/opt/mssql/backup
```

```
[Non-Windows]
docker cp ./AutoLotDocker.bak AutoLot:var/opt/mssql/backup
```

请注意，源目录结构与主机机器匹配（在我示例中为 Windows），而目标则是容器名称然后是目录路径（以目标操作系统格式表示）。

## 使用 SSMS 恢复数据库

要使用 SSMS 恢复数据库，请在对象资源管理器中右键单击数据库节点。选择还原数据库。选择设备并单击省略号。这将打开选择备份设备对话框。

## 恢复数据库到 SQL Server（Docker）

保持“备份媒体类型”设置为文件，然后单击添加，导航到容器中的 AutoLotDocker.bak 文件，然后单击确定。当您返回主还原屏幕时，单击确定，如图 21-6 所示。

![](img/587021f4d5ba039be70a11107130377b_818_0.png)

图 21-6. 使用 SSMS 恢复数据库

## 恢复数据库到 SQL Server（Windows）

保持“备份媒体类型”设置为文件，然后单击添加，导航到 AutoLotWindows.bak，然后单击确定。当您返回主还原屏幕时，单击确定，如图 21-7 所示。

![](img/587021f4d5ba039be70a11107130377b_819_0.png)

图 21-7. 使用 SSMS 恢复数据库

## 使用 Azure Data Studio 恢复数据库

要使用 Azure Data Studio 恢复数据库，请单击查看，选择命令调色板（或按 Ctrl+Shift+P），然后选择还原。选择“备份文件”作为“还原自”选项，然后选择您刚刚复制的文件。目标数据库和相关字段将为您填充，如图 21-8 所示。

![](img/587021f4d5ba039be70a11107130377b_820_0.png)

图 21-8. 使用 Azure Data Studio 将数据库恢复到 Docker

> 注意 使用 Azure Data Studio 恢复 Windows 版本备份的过程相同。只需调整文件名和路径即可。

## 创建 AutoLot 数据库

本节的全部内容都是关于使用 Azure Data Studio 创建 AutoLot 数据库。如果您使用 SSMS，可以使用此处讨论的 SQL 脚本或使用 GUI 工具按照这些步骤进行操作。如果您已恢复备份，可以跳到“ADO.NET 数据提供程序工厂模型”部分。

> 注意 所有脚本文件都位于 Git 仓库中与本章代码一起的名为 Scripts 的文件夹中。

## 创建数据库

要创建 AutoLot 数据库，请使用 Azure Data Studio 连接到数据库服务器。通过选择文件 ► 新查询（或按 Ctrl+N）打开新查询并输入以下命令文本：

```
USE [master]
GO
/****** Object:  Database [AutoLot50]    Script Date: 12/20/2020 01:48:05 ******/
CREATE DATABASE [AutoLot]
GO
ALTER DATABASE [AutoLot] SET RECOVERY SIMPLE
GO
```

除了将恢复模式更改为简单外，这会使用 SQL Server 默认设置创建 AutoLot 数据库。单击运行（或按 F5）以创建数据库。

## 创建表

AutoLot 数据库包含五个表：Inventory、Makes、Customers、Orders 和 CreditRisks。

## 创建 Inventory 表

数据库创建后，是时候创建表了。首先是 Inventory 表。打开新查询并输入以下 SQL：

```
USE [AutoLot]
GO
CREATE TABLE [dbo].[Inventory](
    [Id] [int] IDENTITY(1,1) NOT NULL,
    [MakeId] [int] NOT NULL,
    [Color] [nvarchar](50) NOT NULL,
    [PetName] [nvarchar](50) NOT NULL,
    [TimeStamp] [timestamp] NULL,
    CONSTRAINT [PK_Inventory] PRIMARY KEY CLUSTERED
(
    [Id] ASC
) ON [PRIMARY]
)
ON [PRIMARY]
GO
```

单击运行（或按 F5）以创建表。

## 创建 Makes 表

Inventory 表存储对（尚未创建的）Makes 表的外键。创建新查询并输入以下 SQL 以创建 Makes 表：

```
USE [AutoLot]
GO
CREATE TABLE [dbo].[Makes](
[Id] [int] IDENTITY(1,1) NOT NULL,
[Name] [nvarchar](50) NOT NULL,
[TimeStamp] [timestamp] NULL,
CONSTRAINT [PK_Makes] PRIMARY KEY CLUSTERED
(
[Id] ASC
) ON [PRIMARY]
)
ON [PRIMARY]
GO
```

单击运行（或按 F5）以创建表。

## 创建 Customers 表

Customers 表（顾名思义）将包含客户列表。创建新查询并输入以下 SQL 命令：

```
USE [AutoLot]
GO
CREATE TABLE [dbo].[Customers](
[Id] [int] IDENTITY(1,1) NOT NULL,
[FirstName] [nvarchar](50) NOT NULL,
[LastName] [nvarchar](50) NOT NULL,
[TimeStamp] [timestamp] NULL,
CONSTRAINT [PK_Customers] PRIMARY KEY CLUSTERED
(
[Id] ASC
) ON [PRIMARY]
)
ON [PRIMARY]
GO
```

单击运行（或按 F5）以创建 Customers 表。

## 创建 Orders 表

您将使用下一个表 Orders 来表示给定客户订购的汽车。创建新查询，输入以下代码，然后单击运行（或按 F5）：

```
USE [AutoLot]
GO
CREATE TABLE [dbo].[Orders](
[Id] [int] IDENTITY(1,1) NOT NULL,
[CustomerId] [int] NOT NULL,
[CarId] [int] NOT NULL,
[TimeStamp] [timestamp] NULL,
CONSTRAINT [PK_Orders] PRIMARY KEY CLUSTERED
(
[Id] ASC
) ON [PRIMARY]
)
ON [PRIMARY]
GO
```

## 创建 CreditRisks 表

您将使用最后一个表 CreditRisks 来表示被认为有信用风险的客户。创建新查询，输入以下代码，然后单击运行（或按 F5）：

```
USE [AutoLot]
GO
CREATE TABLE [dbo].[CreditRisks](
    [Id] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [CustomerId] [int] NOT NULL,
    [TimeStamp] [timestamp] NULL,
    CONSTRAINT [PK_CreditRisks] PRIMARY KEY CLUSTERED
(
    [Id] ASC
) ON [PRIMARY]
)
ON [PRIMARY]
GO
```

## 创建表关系

本节接下来将为相互关联的表添加外键关系。

## 创建 Inventory 到 Makes 的关系

打开新查询，输入以下 SQL，然后单击运行（或按 F5）：

```
USE [AutoLot]
GO
CREATE NONCLUSTERED INDEX [IX_Inventory_MakeId] ON [dbo].[Inventory]
(
    [MakeId] ASC
) ON [PRIMARY]
GO
ALTER TABLE [dbo].[Inventory] WITH CHECK ADD CONSTRAINT [FK_Make_Inventory] FOREIGN KEY([MakeId])
REFERENCES [dbo].[Makes] ([Id])
GO
ALTER TABLE [dbo].[Inventory] CHECK CONSTRAINT [FK_Make_Inventory]
GO
```

## 创建 Inventory 到 Orders 的关系

打开新查询，输入以下 SQL，然后单击运行（或按 F5）：

```
USE [AutoLot]
GO
CREATE NONCLUSTERED INDEX [IX_Orders_CarId] ON [dbo].[Orders]
(
    [CarId] ASC
) ON [PRIMARY]
GO
ALTER TABLE [dbo].[Orders]  WITH CHECK ADD  CONSTRAINT [FK_Orders_Inventory] FOREIGN KEY([CarId])
REFERENCES [dbo].[Inventory] ([Id])
GO
ALTER TABLE [dbo].[Orders] CHECK CONSTRAINT [FK_Orders_Inventory]
GO
```

## 创建 Orders 到 Customers 的关系

打开新查询，输入以下 SQL，然后单击运行（或按 F5）：

```
USE [AutoLot]
GO
CREATE UNIQUE NONCLUSTERED INDEX [IX_Orders_CustomerId_CarId] ON [dbo].[Orders]
(
    [CustomerId] ASC,
    [CarId] ASC
) ON [PRIMARY]
GO
ALTER TABLE [dbo].[Orders]  WITH CHECK ADD  CONSTRAINT [FK_Orders_Customers] FOREIGN KEY([CustomerId])
REFERENCES [dbo].[Customers] ([Id])
ON DELETE CASCADE
GO
ALTER TABLE [dbo].[Orders] CHECK CONSTRAINT [FK_Orders_Customers]
GO
```

## 创建 Customers 到 CreditRisks 的关系

打开新查询，输入以下 SQL，然后单击运行（或按 F5）：

```
USE [AutoLot]
GO
CREATE NONCLUSTERED INDEX [IX_CreditRisks_CustomerId] ON [dbo].[CreditRisks]
(
    [CustomerId] ASC
) ON [PRIMARY]
GO
ALTER TABLE [dbo].[CreditRisks]  WITH CHECK ADD  CONSTRAINT [FK_CreditRisks_Customers] FOREIGN KEY([CustomerId])
REFERENCES [dbo].[Customers] ([Id])
ON DELETE CASCADE
GO
ALTER TABLE [dbo].[CreditRisks] CHECK CONSTRAINT [FK_CreditRisks_Customers]
GO
```

