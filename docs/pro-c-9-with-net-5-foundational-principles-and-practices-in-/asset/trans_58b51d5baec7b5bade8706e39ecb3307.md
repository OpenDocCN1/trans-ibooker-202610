

自从 .NET 2.0 发布以来，Redmond 的各位好心人已将此确切功能直接构建到 .NET 基础类库中。该功能已针对 .NET Core 进行了重大更新。

你将在稍后立即检查此正式 API；然而，首先你需要创建一个自定义数据库，在本章（以及之后的许多章节）中使用。

## 设置 SQL Server 和 Azure Data Studio

在本章中，你将针对名为 AutoLot 的简单 SQL Server 测试数据库执行查询。遵循本书通篇使用的汽车主题，该数据库将包含五个相互关联的表（Inventory、Makes、Orders、Customers 和 CreditRisks），其中包含代表虚构汽车销售公司信息的各种数据。在深入数据库细节之前，你必须设置 SQL Server 和 SQL Server IDE。

> **注意：** 如果你使用的是基于 Windows 的开发机器并安装了 Visual Studio 2019，你也安装了 SQL Server Express 的一个实例（称为 localdb），可用于本书中的所有示例。如果你愿意使用该版本，请跳至“安装 SQL Server IDE”部分。

### 安装 SQL Server

在本章和本书中许多剩余章节中，你需要访问 SQL Server 的一个实例。如果你使用的是非 Windows 的开发机器并且没有可用的外部 SQL Server 实例，或选择不使用外部 SQL Server 实例，你可以在 Mac 或 Linux 工作站上在 Docker 容器中运行 SQL Server。Docker 也可在 Windows 机器上运行，因此无论你选择何种操作系统，都可以使用 Docker 运行本书中的示例。

### 在 Docker 容器中安装 SQL Server

如果你使用的是非 Windows 的开发机器并且没有可用于示例的 SQL Server 实例，你可以在 Mac 或 Linux 工作站上在 Docker 容器中运行 SQL Server。Docker 也可在 Windows 机器上运行，因此无论你选择何种操作系统，都可以使用 Docker 运行本书中的示例。

> **注意：** 容器化是一个庞大的主题，本书中没有足够的空间深入介绍容器或 Docker 的细节。本书只会涵盖足够的内容，让你能够完成示例。

Docker Desktop 可从 www.docker.com/get-started 下载。下载并安装适用于你工作站的适当版本（Windows、Mac、Linux）（你需要一个免费的 DockerHub 用户账户）。确保在提示时选择 Linux 容器。

> **注意：** 容器选择（Windows 或 Linux）是容器内部运行的操作系统，而不是你工作站的操作系统。

### 拉取镜像并运行 SQL Server 2019

容器基于镜像，每个镜像都是构建最终产品的分层集合。要获取在容器中运行 SQL Server 2019 所需的镜像，请打开命令窗口并输入以下命令：

```
docker pull mcr.microsoft.com/mssql/server:2019-latest
```

一旦将镜像加载到你的机器上，你需要启动 SQL Server。为此，输入以下命令（全部在一行）：

```
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=P@ssw0rd" -p 5433:1433 --name AutoLot -d mcr.microsoft.com/mssql/server:2019-latest
```

前面的命令接受最终用户许可协议，设置密码（在现实生活中，你需要使用强密码），设置端口映射（你机器上的端口 5433 映射到容器中 SQL Server 的默认端口 1433），然后命名容器（AutoLot），最后告知 Docker 使用之前下载的镜像。

> **注意：** 这些不是你用于实际开发想要使用的设置。有关更改 SA 密码的信息以及教程，请访问 https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker?view=sql-server-ver15&pivots=cs1-bash。

要确认它正在运行，请在命令提示符中输入命令 docker ps -a。你将看到如下输出（为简洁省略了一些列）：

```
C:\Users\japik>docker ps -a
CONTAINER ID IMAGE                    STATUS          PORTS
NAMES
347475cfb823 mcr.microsoft.com/mssql/server:2019-latest Up 6 minutes 0.0.0.0:5433->1433/tcp  AutoLot
```

要停止容器，输入 docker stop 34747，其中数字 34747 是容器 ID 的前五个字符。要重新启动容器，输入 docker start 34747，再次根据容器 ID 的开头更新命令。

> **注意：** 你也可以使用容器的名称（本例中为 AutoLot）与 Docker CLI 命令一起使用，例如 docker start AutoLot。请注意，Docker 命令无论操作系统如何都是区分大小写的。

如果你想使用 Docker Dashboard，右键单击 Docker 鲸鱼（在你的系统托盘中）并选择 Dashboard，你应该会看到镜像在端口 5433 上运行。将鼠标悬停在镜像名称上，你将看到停止、启动和删除（其中其他）命令，如图 21-2 所示。

![](img/587021f4d5ba039be70a11107130377b_813_0.png)

图 21-2. Docker Dashboard

### 安装 SQL Server 2019

Visual Studio 2019 安装了一个特殊的 SQL Server 实例（名为 (localdb)\mssqllocaldb）。如果你选择不使用 SQL Server Express LocalDB（或 Docker）并且使用的是 Windows 机器，你可以安装 SQL Server 2019 Developer Edition。SQL Server 2019 Developer Edition 是免费的，可以从这里下载：

```
https://www.microsoft.com/en-us/sql-server/sql-server-downloads
```

如果你有其他版本，你也可以使用该实例与本书一起使用；你只需相应地更改你的连接屏幕。

### 安装 SQL Server IDE

Azure Data Studio 是用于 SQL Server 的新 IDE。它是免费的并且跨平台，因此可在 Windows、Mac 或 Linux 上运行。它可以从这里下载：

```
https://docs.microsoft.com/en-us/sql/azure-data-studio/download-azure-data-studio
```

> **注意：** 如果你使用的是 Windows 机器并且更喜欢使用 SQL Server Management Studio（SSMS），你可以从这里下载最新版本：https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms。

### 连接到 SQL Server

一旦安装了 Azure Data Studio 或 SSMS，就该连接到你的数据库实例了。以下部分涵盖连接到 Docker 或 LocalDb 中的 SQL Server。如果你使用的是 SQL Server 的另一个实例，请相应地更新以下部分中使用的连接字符串。

### 连接到 Docker 容器中的 SQL Server

要连接到在 Docker 容器中运行的 SQL Server 实例，首先确保它已启动并运行。接下来，如图 21-3 所示，在 Azure Data Studio 中单击“Create a connection”。

![](img/587021f4d5ba039be70a11107130377b_814_0.png)

图 21-3. 在 Azure Data Studio 中创建连接

在 Connection Details 对话框中，为 Server 值输入 `.,5433`。点表示当前主机，`5433` 是你在 Docker 容器中创建 SQL Server 实例时指定的端口。输入 sa 作为用户名；密码与你在创建 SQL Server 实例时输入的相同。名称是可选的，但允许你在后续的 Azure Data Studio 会话中快速选择此连接。图 21-4 显示了这些连接选项。

```text
# Connection

Recent Connections   Saved Connections   Browse (Preview)

Clear List

# Connection Details

Connection type
Microsoft SQL Server

Server
.,5433

Authentication type
SQL Login

User name
sa

Password
********

Remember password

Database
<Default>

Server group
<Default>

Name (optional)
AutoLot

Advanced...

Connect   Cancel
```

图 21-4. 为 Docker SQL Server 设置连接选项

### 连接到 SQL Server LocalDb

要连接到 Visual Studio 安装的 SQL Server Express LocalDb 版本，请将连接信息更新为如图 21-5 所示的内容。

```text
# Connection

Recent Connections
Saved Connections
Browse (Preview)
Clear List

# Connection Details

Connection type
Microsoft SQL Server

Server
(localdb)\mssqllocaldb

Authentication type
Windows Authentication

User name

Password

Remember password

Database
<Default>

Server group
<Default>

Name (optional)
AutoLot-LocalDB

Advanced...

Connect
Cancel
```

图 21-5. 为 SQL Server LocalDb 设置连接选项

连接到 LocalDb 时，你可以使用 Windows 身份验证，因为实例在与 Azure Data Studio 相同的机器上运行，并且与当前登录用户的相同安全上下文。

