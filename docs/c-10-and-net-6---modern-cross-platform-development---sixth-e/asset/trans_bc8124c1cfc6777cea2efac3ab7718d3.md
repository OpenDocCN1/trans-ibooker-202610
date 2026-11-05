
## 为 SQL Server 创建 Northwind 示例数据库

现在我们可以运行一个数据库脚本来创建 Northwind 示例数据库：

- 1. 如果你之前尚未下载或克隆本书的 GitHub 仓库，请现在使用以下链接进行操作：https://github.com/markjprice/cs10dotnet6/。
- 2. 从本地 Git 仓库中的以下路径复制用于为 SQL Server 创建 Northwind 数据库的脚本：/sql-scripts/Northwind4SQLServer.sql 到 WorkingWithEFCore 文件夹中。
- 3. 启动 SQL Server Management Studio。
- 4. 在“连接到服务器”对话框中，对于“服务器名称”，输入 .（一个点）表示本地计算机名称，然后单击“连接”。
- 5. 导航到 文件 | 打开 | 文件....。
- 6. 浏览以选择 Northwind4SQLServer.sql 文件，然后单击“打开”。
- 7. 在工具栏中，单击“执行”，并注意“命令执行成功”消息。
- 8. 在“对象资源管理器”中，展开 Northwind 数据库，然后展开“表”。
- 9. 右键单击 Products，单击“选择前 1000 行”，并注意返回的结果，如图 10.2 所示：

> 如果你必须创建命名实例，例如 cs10dotnet6，则输入 .\cs10dotnet6

![](img/c63b00dca7192cc58cfb9e1f1e64f483_442_0.png)

图 10.2：SQL Server Management Studio 中的 Products 表

- 10. 在“对象资源管理器”工具栏中，单击“断开连接”按钮。
- 11. 退出 SQL Server Management Studio。

## 使用 Server Explorer 管理 Northwind 示例数据库

我们不必使用 SQL Server Management Studio 来执行数据库脚本。我们还可以使用 Visual Studio 中的工具，包括 SQL Server 对象资源管理器和 Server Explorer：

- 1. 在 Visual Studio 中，选择 查看 | Server Explorer。
- 2. 在 Server Explorer 窗口中，右键单击“数据连接”并选择“添加连接...”。
- 3. 如果看到“选择数据源”对话框，如图 10.3 所示，选择 Microsoft SQL Server 然后单击“继续”：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_443_0.png)

图 10.3：选择 SQL Server 作为数据源

- 4. 在“添加连接”对话框中，将服务器名称输入为 .，将数据库名称输入为 Northwind，然后单击“确定”。
- 5. 在 Server Explorer 中，展开数据连接及其表。你应该会看到 13 个表，包括 Categories 和 Products 表。
- 6. 右键单击 Products 表，选择“显示表数据”，并注意返回了 77 行产品。
- 7. 要查看 Products 表列和类型的详细信息，右键单击 Products 并选择“打开表定义”，或在 Server Explorer 中双击该表。

## 使用 SQLite

SQLite 是一个小型、跨平台、自包含的关系数据库管理系统，可在公共领域获得。它是 iOS（iPhone 和 iPad）和 Android 等移动平台上最常见的 RDBMS。即使你使用 Windows 并在上一节中设置了 SQL Server，你可能也想设置 SQLite。我们编写的代码适用于两者，比较细微差异也很有趣。

### 为 macOS 设置 SQLite

SQLite 已包含在 macOS 的 /usr/bin/ 目录中，作为名为 sqlite3 的命令行应用程序。

### 为 Windows 设置 SQLite

在 Windows 上，我们需要将 SQLite 文件夹添加到系统路径中，以便在命令提示符或终端中输入命令时可以找到它：

- 1. 启动你喜欢的浏览器并导航到以下链接：https://www.sqlite.org/download.html。
- 2. 向下滚动页面到 **Windows 预编译二进制文件** 部分。
- 3. 单击 **sqlite-tools-win32-x86-3360000.zip**。请注意，本书出版后该文件可能具有更高的版本号。
- 4. 将 ZIP 文件解压到名为 `C:\Sqlite\` 的文件夹中。
- 5. 导航到 **Windows 设置**。
- 6. 搜索环境并选择 **编辑系统环境变量**。在非英语版本的 Windows 上，请搜索你本地语言中的等效词以找到该设置。
- 7. 单击 **环境变量** 按钮。
- 8. 在 **系统变量** 中，在列表中选择 **路径**，然后单击 **编辑...**。
- 9. 单击 **新建**，输入 `C:\Sqlite`，并按 *Enter*。
- 10. 单击 **确定**。
- 11. 单击 **确定**。
- 12. 单击 **确定**。
- 13. 关闭 **Windows 设置**。

### 为其他操作系统设置 SQLite

SQLite 可以从以下链接下载并安装到其他操作系统：https://www.sqlite.org/download.html。

## 为 SQLite 创建 Northwind 示例数据库

现在我们可以使用 SQL 脚本来为 SQLite 创建 Northwind 示例数据库：

- 1. 如果你之前尚未克隆本书的 GitHub 仓库，请现在使用以下链接进行操作：https://github.com/markjprice/cs10dotnet6/。
- 2. 从本地 Git 仓库中的以下路径复制用于为 SQLite 创建 Northwind 数据库的脚本：/sql-scripts/Northwind4SQLite.sql 到 WorkingWithEFCore 文件夹中。
- 3. 在 WorkingWithEFCore 文件夹中启动命令行：
  - 1. 在 Windows 上，启动文件资源管理器，右键单击 WorkingWithEFCore 文件夹，并选择“在此文件夹中打开命令提示符”或“在 Windows 终端中打开”。
  - 2. 在 macOS 上，启动 Finder，右键单击 WorkingWithEFCore 文件夹，并选择“在此文件夹中新建终端”。
- 4. 输入命令使用 SQLite 执行 SQL 脚本并创建 Northwind.db 数据库，如以下命令所示：
- 5. 请耐心等待，因为此命令可能需要一段时间来创建数据库结构。最终，你将看到 SQLite 命令提示符，如以下输出所示：
- 6. 在 Windows 上按 Ctrl + C 或在 macOS 上按 Ctrl + D 退出 SQLite 命令模式。
- 7. 保持终端或命令提示符窗口打开，因为你很快会再次使用它。

```
sqlite3 Northwind.db --init Northwind4SQLite.sql
```

```
-- Loading resources from Northwind4SQLite.sql
SQLite version 3.36.0 2021-08-24 15:20:15
Enter ".help" for usage hints.
sqlite>
```

## 使用 SQLiteStudio 管理 Northwind 示例数据库

你可以使用名为 SQLiteStudio 的跨平台图形数据库管理器轻松管理 SQLite 数据库：

- 1. 导航到以下链接 https://sqlitestudio.pl，下载并解压应用程序到你喜欢的位置。
- 2. 启动 SQLiteStudio。
- 3. 在“数据库”菜单上，选择“添加数据库”。
- 4. 在“数据库”对话框中，在“文件”部分，单击黄色文件夹按钮浏览本地计算机上的现有数据库文件，选择 WorkingWithEFCore 文件夹中的 Northwind.db 文件，然后单击“确定”。
- 5. 右键单击 Northwind 数据库并选择“连接到数据库”。你将看到脚本创建的 10 个表。（SQLite 的脚本比 SQL Server 的简单；它不会创建那么多表或其他数据库对象。）
- 6. 右键单击 Products 表并选择“编辑表”。
- 7. 在表编辑器窗口中，注意 Products 表的结构，包括列名、数据类型、键和约束，如图 10.4 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_446_0.png)

图 10.4：SQLiteStudio 中的表编辑器显示 Products 表的结构

- 8. 在表编辑器窗口中，单击“数据”选项卡，你将看到 77 个产品，如图 10.5 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_446_1.png)

图 10.5：数据选项卡显示 Products 表中的行

- 9. 在“数据库”窗口中，右键单击 Northwind 并选择“从数据库断开连接”。
- 10. 退出 SQLiteStudio。

## 设置 EF Core

在深入探讨使用 EF Core 管理数据的实际操作之前，让我们简要谈谈 EF Core 数据提供程序之间的选择。

### 选择 EF Core 数据库提供程序

要管理特定数据库中的数据，我们需要能够高效地与该数据库通信的类。

EF Core 数据库提供程序是一组针对特定数据存储优化的类。甚至还有一个提供程序用于将数据存储在当前进程的内存中，这对于高性能单元测试很有用，因为它避免了访问外部系统。

它们作为 NuGet 包分发，如下表所示：

| 要管理的数据存储 | 安装此 NuGet 包 |
|---|---|
| Microsoft SQL Server 2012 或更高版本 | Microsoft.EntityFrameworkCore.SqlServer |
| SQLite 3.7 或更高版本 | Microsoft.EntityFrameworkCore.SQLite |
| MySQL | MySQL.Data.EntityFrameworkCore |
| 内存中 | Microsoft.EntityFrameworkCore.InMemory |
| Azure Cosmos DB SQL API | Microsoft.EntityFrameworkCore.Cosmos |
| Oracle DB 11.2 | Oracle.EntityFrameworkCore |

你可以根据需要在一个项目中安装任意多个 EF Core 数据库提供程序。每个包都包含共享类型以及提供程序特定的类型。

### 连接到数据库

要连接到 SQLite 数据库，我们只需要知道数据库文件名，使用参数 Filename 设置。

要连接到 SQL Server 数据库，我们需要知道多个信息，如下列表所示：

- 服务器名称（如果有实例则包括实例）。
- 数据库名称。
- 安全信息，例如用户名和密码，或是否应自动传递当前登录用户的凭据。

我们在连接字符串中指定这些信息。

为了向后兼容，我们可以在 SQL Server 连接字符串中使用多个可能的关键字来指定各种参数，如下列表所示：
