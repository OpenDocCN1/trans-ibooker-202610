
+   32. 找到 `PersonDataInput_ProcessInputRow()` 方法，并在其定义中添加以下命令：
+   
+   ```csharp
+   Row.IsValidEmail = this.IsValidEmail(Row.EmailAddress);
+   ```
+   
+   需要对每一行调用 `IsValidEmail()` 函数，因为需要验证从源中提取的每个电子邮件地址。

+   33. 保存 VSTA 项目，并通过从“**生成**”菜单中选择“**生成**”命令来生成它。
+   
+   如果所有代码输入正确，生成应成功；否则，你需要按照“错误”窗格中的错误消息来解决问题。

+   34. 生成成功后，关闭 VSTA 窗口，返回 SSDT。

+   35. 在“脚本转换编辑器”中，单击“**确定**”，确认并保存组件脚本，然后保存包。

+   36. 从 SSIS 工具箱中，将一个“**条件拆分**”转换拖到数据流设计器中，并将其名称更改为 `Valid or Invalid Email`。

+   37. 将常规数据路径从“**验证电子邮件**”转换连接到“**条件拆分**”转换。

+   38. 双击新添加的转换，打开“**条件拆分转换编辑器**”。

+   39. 使用以下设置创建新输出：
+   
+   - **属性：** `Value`
+   - **输出名称：** `Valid Email`
+   - **条件：** `IsValidEmail`
+   
+   或者，为了减少输入，可以将 `IsValidEmail` 列从“**条件拆分转换编辑器**”左上角的“**列**”列表拖出。

40. 将默认输出名称更改为“无效电子邮件”。

请参考以下截图来验证你的设置：

![](img/07d1225e179d5df9f2b2d26b026386c6_524_0.png)

+   41. 单击“**确定**”完成配置。
+   42. 从 SSIS 工具箱中，将一个“**多播**”转换拖到数据流设计器中，并将常规数据路径从“**有效**或**无效电子邮件**”转换连接到它。
+   43. 当“**输入输出选择**”对话框提示时，在“**输出**”选择框中选择“**无效电子邮件**”，如下面的截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_525_0.png)

+   44. 单击“**确定**”确认选择。
+   45. 在“**无效电子邮件**”数据路径上启用“**数据查看器**”。
+   46. 保存包并在调试模式下执行。观察执行过程并检查放置在“**无效电子邮件**”数据路径中的行。
+   47. 停止调试模式执行；保持解决方案打开，因为下一个配方中需要用到它。

## 使用脚本组件作为源

在本配方中，你将使用脚本组件作为数据流源来创建自定义源。

你将连接到一个 Web 服务并从中检索数据，然后将数据放入数据流管道中。

### 操作步骤...

+   1. 向本章开头创建的“**AdventureWorksETL**”项目添加一个新的 SSIS 包。
+   2. 将新创建的包名称更改为 `CustomWebServiceSource.dtsx` 并保存。
+   3. 确保“**CustomWebServiceSource.dtsx**”包处于活动状态，然后使用以下设置创建一个新的包参数：
      - **属性：** `Value`
      - **名称：** `CountryName`
      - **数据类型：** `String`
      - **值：** `France`
+   4. 将一个“**数据流**”任务拖到控制流设计器中，并将其名称更改为“机场信息”。
+   5. 在数据流设计器中打开“**机场信息**”任务，并将一个“**脚本组件**”拖到数据流设计器中。
+   6. 在“**选择脚本组件类型**”对话框中，选择“**源**”，如下面的截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_527_0.png)

+   7. 单击“**确定**”确认选择。
+   8. 将脚本组件的名称更改为“按国家/地区的机场信息”。
+   9. 双击“**按国家/地区的机场信息**”源，打开脚本转换编辑器。
+   10. 在“**脚本**”页面上，将 `$Package::CountryName` 参数添加到 `ReadOnlyVariables` 集合中，并确保“**脚本语言**”属性选择的是 Microsoft Visual C# 2015。
+   11. 在“**输入和输出**”页面上，选择“**输出 0**”，并将其名称更改为 `AirportInfoOutput`。
+   12. 使用以下设置向输出的“列集合”中添加列：

```
Name DataType Length
Country Unicode string [DT_WSTR] 50
AirportName Unicode string [DT_WSTR] 50
AirportCode Unicode string [DT_WSTR] 3
RunwayLengthFeet four-byte signed integer [DT_I4] /
```

请参考以下截图来验证你的设置：

![](img/07d1225e179d5df9f2b2d26b026386c6_528_0.png)

💡 **提示**

要确保每个新列都添加到列表的末尾，请在添加下一列之前单击“**输出列**”节点。

+   13. 单击“确定”确认配置。
+   14. 再次打开“**按国家/地区的机场信息**”转换编辑器，这次单击“**编辑脚本...**”以启动 VSTA。

+   15. 在“**解决方案资源管理器**”中，右键单击“**引用**”节点，并从快捷菜单中选择“**添加服务引用...**”，如下面的截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_529_0.png)

+   16. 在“**添加服务引用**”对话框中，单击对话框左下角的“**高级...**”。

+   17. 在“**服务引用设置**”对话框中，单击“**添加 Web 引用...**”以打开“**添加 Web 引用**”对话框。

+   18. 在“**URL**”文本框中输入以下 URL：

```
http://www.webservicex.net/airport.asmx?WSDL
```

+   19. 单击文本框右侧指向右方的箭头，以加载来自 Web 服务的信息。

20. 在“Web 引用名称”文本框中输入 AirportInfo。

请参考以下截图来验证你的设置：

![](img/07d1225e179d5df9f2b2d26b026386c6_530_0.png)

21. 单击“添加引用”以确认设置并创建引用。

22. 保存项目。

23. 在组件定义中，在代码开头找到“命名空间”区域，并展开它以列出现有引用。

24. 在最后一个 using 指令之后添加另一个 using 指令，然后开始输入项目名称（应与 SC_04df03808e054a509e49d4cce022812c 类似）；让 Visual Studio 的 IntelliSense 提示完整的命名空间，然后完成对 AirportInfo 命名空间的引用：

```
using SC_04df03808e054a509e49d4cce022812c.AirportInfo;
```

+   25. 此示例中的 `SC_04df03808e054a509e49d4cce022812c` 值用于 VSTA 项目的名称及其默认命名空间。当你向项目添加 Web 引用时，会自动创建代理类和函数，并将它们放置在项目的默认命名空间中。

+   26. 添加以下引用，因为稍后将需要用到它：

+   ```csharp
+   using System.Xml;
+   ```
+   
+   27. 从“**生成**”菜单中选择“**生成**”，以保存并生成项目。

+   28. 如果正确执行了前面的步骤，项目应能成功生成；否则，请检查“错误”窗格中报告的错误并进行必要的更正。

+       1. 如果自动生成的代码返回错误，请在“**解决方案资源管理器**”中右键单击 Web 引用，并从快捷菜单中选择“**删除**”，然后重新生成项目并重复步骤 14 到 21。

+   29. 将以下变量声明添加到组件定义的 `ScriptMain` 类中：

+   ```csharp
+   private String _countryName;
+   private String _airportXPath = @"/NewDataSet/Table";
+   private XmlNodeList _airportXmlNodes;
+   ```
+   
+   30. 将以下命令添加到 `PreExecute()` 方法中：

+   ```csharp
+   airport airportInfo = new airport();
+   String airportInfoByCountry =
+   airportInfo.GetAirportInformationByCountry(_countryName);
+   XmlDocument airportInfoByCountryXml = new XmlDocument();
+   airportInfoByCountryXml.LoadXml(airportInfoByCountry);
+   _airportXmlNodes = airportInfoByCountryXml.SelectNodes(_airportXPath);
+   ```
+   
+   31. 给定 Web 服务的 `GetAirportInformationByCountry` 函数会针对所提供的国家/地区名称返回机场列表及其属性。此函数每次数据流执行只需调用一次；因此，应将其放在 `PreExecute()` 方法中，该方法在数据流执行开始时仅调用一次。

