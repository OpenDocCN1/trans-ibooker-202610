

## 将 Power BI 与其他应用程序集成

- 3. 在 **现有连接** 对话框中，选择 Power BI 连接并选择 **打开** 按钮。
- 4. 在 **导入数据** 对话框中，选择 **数据透视表报表** 然后选择 **确定** 按钮。
- 5. 创建一个简单的数据透视表报表，包含一个度量值和一个属性，例如按 **产品类别** 的 **Internet Net Sales**。

    ![](img/0b67f3eabca89f1e12f62eac78fb9ffb_606_0.png)

    图 13.19：基于 Power BI 服务数据集的 Excel 数据透视表

- 6. 双击其中一个度量值单元格，例如 **$339,773** 以执行“下钻”查询。底层 **Internet Sales** 事实表的全部列将被检索，并按 **Clothing** 类别进行筛选。

    ![](img/0b67f3eabca89f1e12f62eac78fb9ffb_606_1.png)

    图 13.20：下钻的 Excel 表结果

- 7. 在 Excel 表中选择一个单元格，右键单击并选择 **表格** 然后 **编辑查询**...

    ![](img/0b67f3eabca89f1e12f62eac78fb9ffb_606_2.png)

    图 13.21：Excel 表格选项

- 8. 在 **命令文本** 区域中，输入（或粘贴）自定义 DAX 查询并单击 **确定。**

    ![](img/0b67f3eabca89f1e12f62eac78fb9ffb_607_0.png)

    图 13.22：从 DAX Studio 粘贴到编辑 OLE DB 查询对话框的命令文本区域中的 DAX 查询

- 9. 如果查询有效，Excel 表将更新以返回查询中指定的列。
- 10. 可视化对象，例如地图可视化对象，可以使用此表（DAX 查询）作为其数据源。

    ![](img/0b67f3eabca89f1e12f62eac78fb9ffb_607_1.png)

    图 13.23：DAX 查询的 Excel 表结果（左）和 Excel 地图可视化对象（右）

可在 **连接属性** 中的 **OLAP 下钻** 属性中调整要检索的行数。可视化对象上应用了自定义数据标签格式，以千为单位并保留一位小数表示值。请注意，默认数据透视表不能用作此可视化对象或其他多个新 Excel 可视化对象的源。

### 另请参阅

- Analyze in Excel: https://bit.ly/3bIMSeM
- CUBEVALUE function: https://bit.ly/3DfftUA
- CUBEMEMBER function: https://bit.ly/3AXr1Kq

## 将 Power BI 报表构建到 PowerPoint 演示文稿中

Microsoft PowerPoint 仍然是标准的幻灯片演示应用程序，而将外部工具中的数据分析和可视化集成到演示文稿中通常是有效演示文稿的重要组成部分。为响应大量客户请求，Power BI 服务支持将 Power BI 报表导出为 PowerPoint 文件的功能。Power BI 报告的每个页面都会转换为独立的 PowerPoint 幻灯片，并且 Power BI 服务会根据报表和相关元数据（例如最后刷新日期）创建标题页。存在一些当前限制，例如导出的文件是静态的以及支持的可视化对象，但该功能可供所有 Power BI 用户使用，以简化演示文稿幻灯片的创建。然而，更深层次的集成计划在未来实现，包括将动态 Power BI 报表嵌入到 PowerPoint 中的能力。

本食谱包含一个准备过程，以更好地利用导出到 PowerPoint 的功能并避免当前限制。此外，还描述了一个用户从已发布的应用程序导出 Power BI 报表并在 PowerPoint 中访问内容的示例过程。

### 准备就绪

为准备本食谱，请执行以下步骤：

- 1. 登录到 Power BI 服务。
- 2. 单击右上角的齿轮图标并选择 **管理门户**。
- 3. 单击 **租户设置**。
- 4. 在 **导出和共享** 下，在 Power BI 管理门户中启用 **导出报表为 PowerPoint 演示文稿或 PDF 文档** 功能：

    ![](img/0b67f3eabca89f1e12f62eac78fb9ffb_609_0.png)

    *图 13.24：Power BI 管理门户中的租户设置*

    如前面的屏幕截图所示，Power BI 管理员或 Office 365 全局管理员还可以将该功能限制为特定的安全组。

- 5. 确定将作为要创建的 PowerPoint 的来源的 Power BI 报表及其数据集。
- 6. 如果报表包含多个页面，请统计报表页面的数量。目前超过 15 页的报表无法导出。
- 7. 确定报表中是否有任何不受支持的可视化对象，包括 R 可视化对象和未通过认证的自定义可视化对象。
- 8. 检查报表可视化对象中是否使用了任何背景图像或是否设置了任何自定义页面大小。最好避免使用背景图像和自定义页面大小。
- 9. 根据之前的步骤和导出的初始测试，对现有报表进行修订或创建单独的报表（以当前报表为起点），该报表将专门用于 PowerPoint。

现在您可以完成本食谱的步骤。

### 如何操作...

要实施本食谱，请执行以下操作：

- 1. 在 Power BI 服务中打开报表。
- 2. 从报表标题中选择导出，然后选择 PowerPoint。

    ![](img/0b67f3eabca89f1e12f62eac78fb9ffb_610_0.png)

    图 13.25：导出到 PowerPoint

- 3. 导出对话框提供选项，单击导出按钮。

    ![](img/0b67f3eabca89f1e12f62eac78fb9ffb_610_1.png)

    图 13.26：PowerPoint 的导出选项

- 4. 消息将指示导出正在进行中，可能需要几分钟。
- 5. 打开 PowerPoint 文件并在 PowerPoint 中根据需要进行额外调整。

    ![](img/0b67f3eabca89f1e12f62eac78fb9ffb_610_2.png)

    图 13.27：Microsoft PowerPoint 幻灯片排序视图中的已导出 Power BI 报表

正如您所见，导出到 PowerPoint 非常容易！

### 其工作原理...

根据浏览器及其下载设置，文件将下载到特定路径，或浏览器会显示保存或打开 PowerPoint 文件的消息。导出过程会自动生成标题页，其中包含报表名称和指向 Power BI 服务中报表的链接。标题页还包括最后数据刷新和下载日期时间值。每个报表页面都会转换为包含报表页面高质量渲染图像的幻灯片。针对可视化对象指定的任何替代文本都包含在每个页面的备注中。

与执行仪表板等高可见性内容的其他规划和预防措施类似，了解和确信数据源、刷新过程、数据质量和所有权非常重要。例如，如果源数据集从多个来源检索，包括临时 Excel 文件，并且刷新失败的历史记录，则该报表可能不是 PowerPoint 演示文稿的良好候选者。基于已验证的 IT 管理的 Analysis Services 模型并且拥有明确所有者的报表将是更好的选择。

背景图像将被图表的边界区域裁剪，因此建议删除或避免背景图像。此外，导出的报表页面始终会生成标准 16:9 PowerPoint 幻灯片大小，因此应避免自定义或非标准页面大小。用于提供自定义分组、边框和可视化对象背景色的形状（例如矩形和线条）也可能需要删除以实现正确的 PowerPoint 渲染。最后，在使用导出到 PowerPoint 的报表中应避免使用未认证的可视化对象、R 可视化对象和 Python 可视化对象。

### 更多内容...

Power BI 服务支持通过 Power BI REST API 将报表导出为 PowerPoint 或 PDF 以及图像文件。此外，分页报表还可以导出为以下其他格式：

- MHTML
- Microsoft Word
- XML

AppSource 中提供了一个第三方加载项，用于将 Power BI 服务中的 Power BI 瓷砖集成到 Microsoft Office 文档中，名为 Power BI Tiles：https://bit.ly/3w2sUmq。DevScope 的产品包括自动化的 Office 到 Power BI 刷新流程，并支持 Word、Outlook 和 PowerPoint。

