

进入暂存区后，数据将被复制到星型模式数据库表示中。表位于同一数据库中，但具有单独的架构，称为 DW。以下图表显示了 DW 表：

![](img/07d1225e179d5df9f2b2d26b026386c6_424_0.png)

数据仓库的目标是简化数据消费。大多数用户都能理解，并且数据被分类到代表最终用户将基于其进行分析的主题的区域（表）中。

客户信息已被重新分组到两个表中，`DimCustomer` 和 `DimAddress`。`DimAddress` 表与事实表有两个链接。这些关系代表了多个地址，在我们的案例中为两个：客户的账单地址和发货地址。

产品信息已被展平为一个维度：`DimProduct`。尽管基础模型允许产品描述有多种语言，但在维度中仅保留两种：法语和英语 - `EnglishDescription` 和 `FrenchDescription`。

订单表已合并为一个事实表：`FactOrders`。已添加 `DimTime` 维度以允许使用各种日期更好地查询订单：订单日期、发货日期和到期日期。`SalesOrderNumber` 和 `PurchaseOrderNumber` 被视为派生维度并保留在事实表中。我们没有足够的信息可以从这些列中派生，并且它们与事实紧密相关。

剩余的维度 `DimOrderProvenance` 在 AdventureWorksLT 操作数据库中没有来源。它已被添加并由 Master Data Services 管理，这是随 SQL Server 2016 Developer Edition 提供的另一项服务。我们将在本书后面讨论此服务和此维度。

在接下来的几个配方中，我们将部署这些数据库以及加载这些表的 ETL（SSIS 包）。

## 安装示例解决方案

本节将指导您部署示例解决方案中包含的数据库。

## 准备工作

本节假设您已下载解决方案文件。

## 如何操作...

- 1. 我们首先创建一个将保存解决方案文件的目录。在我们的案例中，我们创建了一个名为 `C:\Projects\SSISCookbook` 的文件夹，并在其中解压缩解决方案文件，如以下屏幕截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_426_0.png)

- 2. 现在，我们通过双击 `SSISCookBook` 解决方案文件打开解决方案，如前一屏幕截图所示突出显示。
- 3. 解决方案打开，您将看到其项目，如以下屏幕截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_426_1.png)

## 如何操作...

- 1. 在 SQL Server Data Tools（Visual Studio）中，右键单击 DB AdventureWorksLT 项目并选择 **Publish...**，如以下屏幕截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_428_0.png)

2. 出现以下屏幕；单击 **Edit...** 按钮以创建到本地 SQL Server 实例的连接字符串：

![](img/07d1225e179d5df9f2b2d26b026386c6_429_0.png)

3. 如以下屏幕截图所示，单击 Browse 选项卡会弹出一个屏幕，我们可以在其中选择 SQL Server 实例位置。在我们的案例中，我们将选择 Local。展开 Local 标签并选择您的 SQL Server 实例，在我们的案例中为 SSISCOOKBOOK。Server Name 文本框将显示您选择的服务器。单击 OK 返回到上一个屏幕。

![](img/07d1225e179d5df9f2b2d26b026386c6_430_0.png)

5. 返回到 Publish Database 屏幕，按如下方式填写文本框：

- - **Database name:** AdventureWorksLT
- - **Publish script name:** DB.AdventureWorksLT.sql

它也在以下屏幕截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_432_0.png)

6. 单击 Save Profile As... 按钮。

7. 我们将把这些设置保存为此数据库项目的发布配置文件。这将允许我们在未来重用它，而无需重复前面的步骤。按以下屏幕截图所示填写文件名。它将在项目中创建一个发布配置文件。如以下屏幕截图所示，将发布配置文件命名为 DB.AdventureWorksLT 并单击 Save。

![](img/07d1225e179d5df9f2b2d26b026386c6_433_0.png)

8. 返回到 Publish Database 窗口，单击如以下屏幕截图所示的 Publish 按钮。Generate Script 选项用于在我们只想创建部署脚本而不立即部署数据库时使用。一个很好的例子是当我们创建一个脚本来交给负责数据库部署的 DBA 时。

![](img/07d1225e179d5df9f2b2d26b026386c6_433_1.png)

9. **Data Tools Operations** 窗口打开并显示部署数据库时执行的步骤。您应该会看到与以下屏幕截图非常相似的内容：

![](img/07d1225e179d5df9f2b2d26b026386c6_434_0.png)

10. 启动 **SQL Server Management Studio (SSMS)** 并在对象资源管理器中查找 `AdventureWorksLT` 数据库，如以下屏幕截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_434_1.png)

11. 右键单击任意表并选择 **Select Top 1000 Rows**，如以下屏幕截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_435_0.png)

此文件包含加载不同表的 INSERT 语句。您可以在以下屏幕截图中一窥其内容：

![](img/07d1225e179d5df9f2b2d26b026386c6_437_0.png)

此 SQL 文件通过 SSDT 中的特殊脚本调用：部署后脚本。要创建它，我们只需在项目中添加一个脚本，如以下屏幕截图所示。当我们右键单击文件夹并选择 Add | New | Script 时会出现此对话框。

![](img/07d1225e179d5df9f2b2d26b026386c6_437_1.png)

## 准备工作

本配方假设您有权访问本书提供的示例解决方案，并且您已阅读并执行了部署源数据库的所有步骤。

## 如何操作...

步骤基本上与部署源数据库时使用的步骤相似。由于我们之前已解释了大部分步骤，本配方将更简洁：

- 1. `DB.AdventureworksLTDW` 项目包含以下架构：
- - **Cube:** 这包含特定的数据库对象，主要是可被 SSAS 多维数据集使用的视图。
- - **dbo:** 这是默认架构。
- - **DW:** 此文件夹包含属于 DW 架构的数据库对象，即数据仓库星型模式对象。
- - **Scripts:** 这是一个保存预部署或部署后脚本以及实用脚本的文件夹。
- - **Staging:** 此文件夹包含由 `Staging` 架构使用的数据库对象。
- - **SystemLog:** 此文件夹包含由 SSIS 框架使用的数据库对象。我们将在本书后面的部分讨论它。

它也在以下屏幕截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_439_0.png)

2. 在解决方案中右键单击 DB.AdventureWorksLT DW 项目并选择 **Publish...** 如以下屏幕截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_440_0.png)

3. **Publish Database** 窗口出现。单击 **Load Profile...** 并从出现的 **Publish Settings** 窗口中选择 DB.AdventureWorks2016.publish.publish 发布配置文件。单击 **Open**，如以下屏幕截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_440_1.png)

4. 返回到 **Publish Database** 窗口，单击 **Publish** 以部署数据库，如以下屏幕截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_441_0.png)

5. 数据库开始部署。您可以在屏幕底部的 **Data Tools Operations** 窗口中看到其进度。它应如下屏幕截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_441_1.png)

