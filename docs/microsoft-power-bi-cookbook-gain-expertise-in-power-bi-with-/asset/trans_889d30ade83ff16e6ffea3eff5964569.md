

7. 从下拉列表中为 **本地数据网关** 选择你的网关。
8. 输入你的身份验证信息并单击 **Next** 按钮。
9. 选择 **DimCustomer** 表并单击 **Transform data** 按钮。
10. 展开 **DimGeography** 列以显示以下列，并取消选中 **Use original column name as prefix**：
    - ☐ City
    - ☐ StateProvinceName
    - ☐ EnglishCountryRegionName
    - ☐ PostalCode
11. 单击 **Next** 按钮。
12. 选择 **Load to existing table** 并选择 **Contact** 作为 **Table display name**。
13. 创建以下映射：
    - ☐ City = Address1_City
    - ☐ EnglishCountryRegionName = Address1_Country
    - ☐ AddressLine1 = Address1_Line1
    - ☐ AddressLine2 = Address1_Line2
    - ☐ PostalCode = Address1_PostalCode
    - ☐ StateProvinceName = Address1_StateOrProvince
    - ☐ Phone = Address1_Telephone1
    - ☐ YearlyIncome = AnnualIncome
    - ☐ BirthDate = BirthDate
    - ☐ FirstName = FirstName
    - ☐ Title = JobTitle
    - ☐ LastName = LastName
    - ☐ TotalChildren = NumberOfChildren
    - ☐ Phone = Telephone1
14. 将所有其他列设置为 **(none)**，然后单击 **Next** 按钮。
15. 保留 **Refresh manually** 的默认值并单击 **Create** 按钮。
16. 刷新将自动开始，请等待刷新完成。
17. 创建一个新的 Power BI Desktop 文件。
18. 选择 **Get data** 然后选择 **Dataverse**。
19. 输入来自 *Getting started 的第 10 步* 的 **Environment URL**。
20. 登录并单击 **Connect** 按钮。
21. 在 **Navigator** 中选择 **Contact** 并单击 **Load** 按钮。
22. 创建一个简单的报表视觉对象以显示加载到 **Dataverse** 环境中的联系人：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_629_0.png)

图 13.50：Dataverse 环境中的客户

*图 13.50* 显示了加载到 Dataverse 的客户位置。

### How it works…

Power Apps 数据流使用与 Power BI 相同的 Power Query 技术。这些数据流可以从许多不同的源摄取数据，包括通过数据网关的本地源以及基于云的源。一旦数据导入 Dataverse，此数据可以通过导入或 DirectQuery 由 Power BI 访问。

重要的是要指出，Power Apps 和 Power Automate 也可以原生访问存储在 Dataverse 环境中的数据。

### There's more…

除了 Dataverse 之外，Power BI 还与 Dynamics 365 应用程序具有原生集成，允许 Power BI 仪表板在 Dynamics 365 应用程序内显示。为演示此功能，请执行以下操作：

1. 打开 Power Platform 管理中心，`https://admin.powerplatform.microsoft.com`
2. 在左侧导航窗格中，单击 **Environments**
3. 单击在此食谱中创建的环境
4. 单击 **Environment URL** 的链接以打开环境
5. 选择右上角的齿轮图标并选择 **Advanced Settings**
6. 从左上角的 **Settings** 下拉菜单中选择 **Administration:**

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_630_0.png)

图 13.51：Dynamics 365 设置

7. 选择 **System Settings**
8. 单击 **Reporting** 选项卡并将 **Allow Power BI visualization embedding** 设置为 **Yes**
9. 单击 **OK** 按钮
10. 打开环境的 Sales Hub 应用并单击 **Dashboards**
11. 选择 **New** 然后选择 **Power BI Dashboard**
12. 如有必要，登录到 Power BI
13. 选择一个 **Workspace** 和一个 **Dashboard** 并单击 **Save** 按钮
14. 选择 New 然后选择 **Dynamics 365 Dashboard**
15. 选择一个布局然后单击 **Create** 按钮
16. 单击 **Add a Power BI tile to the dashboard** 图标：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_630_1.png)

图 13.52：Dynamics 365 中添加到仪表板的 Power BI 磁贴图标

17. 选择一个 **Workspace**、一个 **Dashboard** 和一个 **Tile** 然后单击 **OK** 按钮
18. 为仪表板提供一个 **Name** 并单击 **Save** 然后单击 **Close**

Power BI 仪表板和磁贴继续表现出对其底层报表的链接行为。单击报表磁贴会打开底层报表，并且该报表是交互式的，这意味着单击视觉对象会交叉筛选报表中的其他视觉对象，就像在 Power BI 服务中查看报表一样。

### See also

- Administer Power Platform: https://bit.ly/3bqaKTQ
- Create and use dataflows in Power Apps: https://bit.ly/3eP6tLL

## Connecting Dynamics 365 Finance and Operations and the Power Platform

Power BI 是 Microsoft Power Platform 产品系列的一部分。Power Platform 包括 Power BI、Power Apps、Power Automate 和 Power Virtual Agents。然而，Power Platform 也是一个更大产品系列的一部分，即 Microsoft Business Applications。Microsoft Business Applications 是围绕 Dynamics 365 Sales、Dynamics 365 Business Central 和 Dynamics 365 Finance and Operations 构建的应用程序。Microsoft 实际上将 Business Applications 细分为以下几类：

- Microsoft Power Platform
- Customer Service
- Field Service
- Finance
- Marketing
- Operations
- Commerce
- Human Resources
- Sales

然而，需要注意的是，Customer Service、Field Service、Marketing、Commerce、Human Resources 和 Sales 都是围绕 Microsoft **客户关系管理 (CRM)** 系统构建的，该系统以前称为 Microsoft CRM。可扩展的 Microsoft CRM 系统，有时称为 XRM，实际上是 Dataverse 的核心。Finance and Operations 应用程序指的是 **企业资源规划 (ERP)** 应用程序，即 Dynamics 365 Finance and Operations 或 Dynamics 365 Business Central。请注意，Dynamics 365 Finance and Operations 是基于 Dynamics **AX (Axapta)** 构建的，而 Dynamics 365 Business Central 是基于 Dynamics **NAV (Navision)** 构建的。Dynamics **GP (Great Plains)** 并未过渡到云端。

## Integrating Power BI with Other Applications

Power Platform 管理中心提供了一个中央位置，用于集成 Microsoft Business Applications 组合中的各种产品。这还包括强大的数据集成功能，允许管理员将来自许多不同环境的数据集成到 Dataverse 环境中。与数据流类似，这些数据集成项目促进了数据管理策略和业务信息流的集中控制。

本食谱演示如何使用 Power Platform 管理中心创建数据集成项目，将 Dynamics 365 Finance and Operations 数据与 Dataverse 环境同步，然后从该数据构建 Power BI 报表。

### Getting ready

为准备本食谱，请执行以下步骤：

1. 打开 Web 浏览器，前往 `https://admin.powerplatform.microsoft.com`，并使用你的 Microsoft 365 帐户登录。这是 Power Platform 管理中心。
2. 在左侧导航窗格中，单击 **Environments**。
3. 在功能区区域中，单击 **+ New**。这将在页面右侧打开一个 **New environment** 窗格。
4. 在 **New environment** 窗格中，输入环境的 **Name**，选择 **Type**（**Production、Sandbox** 或 **Trial** 之一），并选择一个 **Region**。此外，将 **Create a database for this environment** 切换为 **Yes** 并单击窗格底部的 **Next** 按钮。
5. 选择 **Language** 和 **Currency**。如有需要提供 **URL**。
6. 将 **Enable Dynamics 365 apps** 切换为 **Yes**。
7. 单击 **Automatically deploy these apps** 的下拉菜单并选中所有复选框。
8. 单击窗格底部的 **Save** 按钮。
9. 单击环境并记下 **Environment URL**。
10. 单击 **Environment URL** 以打开环境。
11. 单击右上角的齿轮图标并选择 **Advanced Settings**。
12. 单击 **Settings** 然后单击 **Security**。
13. 选择 **Teams**。
14. 选中默认团队旁边的复选框并选择 **MANAGE ROLES**。
15. 选中 **System Administrator** 的复选框并单击 **OK** 按钮。

