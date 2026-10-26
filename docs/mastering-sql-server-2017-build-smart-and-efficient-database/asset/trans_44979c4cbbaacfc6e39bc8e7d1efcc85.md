

## 如何操作...

- 1. 在 Visual Studio 2015 中，打开你在上一节 *Designing a custom task* 中创建的 `SSISCustomization.sln` 解决方案；它应位于 `C:\SSIS2016Cookbook\Chapter13\Starter\SSISCustomization\` 文件夹中。如果未遵循上一节，请执行步骤 1 至 3 来创建解决方案。
- 2. 在 **Solution Explorer** 中，右键单击 **Solution** 节点，并从快捷菜单中选择 **Add New Project...** 以添加新项目。
- 3. 在 **Add New Project** 对话框中，选择位于 Visual C# 模板组中的 **Class Library** 模板；使用 `SSISCustomComponents` 作为项目名称。参考以下屏幕截图以验证你的设置：

![](img/07d1225e179d5df9f2b2d26b026386c6_606_0.png)

![](img/07d1225e179d5df9f2b2d26b026386c6_608_0.png)

如果有多个版本的相同程序集可用，请确保仅选中版本 13.0.0.0。

- 15. 单击 **OK** 以确认选择。
- 16. 在 **Solution Explorer** 中，确保 **References** 节点已展开。

17. 选择 Microsoft.SqlServer.DTSPipelineWrap 节点，定位到程序集属性中的 Embed Interop Types 设置，并确保将其设置为 False，如以下屏幕截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_609_0.png)

- 18. 对 `Microsoft.SQLServer.DTSRuntimeWrap` 程序集执行相同的操作。
- 19. 保存项目。
- 20. 确保 `ValidateEmail.cs` 已在设计器中打开，并用以下引用替换定义顶部现有的程序集引用列表：

```
using Microsoft.SqlServer.Dts.Pipeline;
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;
using Microsoft.SqlServer.Dts.Runtime.Wrapper;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using System.Xml;
```

这些引用提供了开发完整的 SSIS 组件以及执行其操作所需的功能。

- 21. 修改 `ValidateEmail` 类定义，使其派生自 `Microsoft.SqlServer.Dts.Pipeline.PipelineComponent` 基类，并实现 `DtsPipelineComponent` 属性：

```
[DtsPipelineComponent
    (
    ComponentType = ComponentType.Transform,
    DisplayName = "Validate Email",
    Description = "Validates email addresses using the
corresponding rule in a data profile file.",
    NoEditor = false
    )]
public class ValidateEmail : PipelineComponent
{
}
```

基类提供了每个 SSIS 组件在设计时和运行时需要实现的功能，而 `DtsPipelineComponent` 属性允许该类被识别为 SSIS 组件——在设计时和运行时均可。

22. 向 `ValidateEmail` 类定义添加以下私有常量声明：

```
private const String DATA_PROFILE_FILE_NAME_PROPERTY_NAME = "Data Profile File Name";
private const String DATA_PROFILE_FILE_NAME_PROPERTY_DESCRIPTION = "Data profile file name (fully qualified).";
private const String DATA_PROFILE_COLUMN_NAME_PROPERTY_NAME = "Data Profile Column Name";
private const String DATA_PROFILE_COLUMN_NAME_PROPERTY_DESCRIPTION = "The name of the columns in data profile.";
private const String INPUT_NAME = "ValidateEmailInput";
private const String INPUT_COLUMN_NAME = "Input Column Name";
private const String INPUT_COLUMN_DESCRIPTION = "The name of the column to be validated.";
private const String OUTPUT_NAME = "ValidateEmailOutput";
private const String IS_VALID_COLUMN_NAME = "IsValidEmail";
private const String IS_VALID_COLUMN_DESCRIPTION = "True, if the value of the selected column is a valid email address; otherwise, False.";
private const String IS_INTERNAL_OBJECT_PROPERTY_NAME = "isInternal";

private const String TOO_MANY_INPUTS_MESSAGE = "Only a single input is supported.";
private const String TOO_MANY_OUTPUTS_MESSAGE = "Only a single output is supported.";
private const String DEFAULT_OUTPUT_MUST_EXIST_MESSAGE = "The built-in synchronous output cannot be removed.";
private const String USER_DEFINED_COLUMNS_NOT_SUPPORTED = "User-defined columns are not supported.";
private const String DEFAULT_COLUMNS_MUST_EXIST = "Built-in output columns cannot be removed";
private const String DATA_PROFILE_FILE_NOT_FOUND_MESSAGE_PATTERN = "The file [{0}] could not be located.";
private const String DATA_PROFILE_FILE_FOUND_MESSAGE_PATTERN = "The file [{0}] exists.";
private const String REGEX_PATTERNS_LOADED_MESSAGE_PATTERN = "{0} Regular Expression patterns loaded.";
private const String DATA_PROFILE_COLUMN_NOT_FOUND_MESSAGE_PATTERN = "The file [{0}] does not contain a column named [{1}].";
private const String REGEX_PATTERNS_NOT_FOUND_MESSAGE_PATTERN = "The file [{0}] does not contain any Regular Expressions patterns data for a column named [{1}].";
private const String INPUT_COLUMN_NOT_SET_MESSAGE = "The input column has not been set.";
private const String INPUT_COLUMN_NOT_FOUND_MESSAGE_PATTERN = "An input column named [{0}] cannot be found.";
```

```
private const String INPUT_COLUMN_FOUND_MESSAGE_PATTERN = "The input column named [{0}] was found.";
private const String INPUT_COLUMN_DATATYPE_NOT_SUPPORTED_MESSAGE_PATTERN = "The data type [{0}] of the selected input column [{1}] is not supported.\r\nPlease, use a column with a supported data type: DT_NTEXT, DT_TEXT, DT_STR, or DT_WSTR.";
```

```
private const String DATA_PROFILE_NAMESPACE = "http://schemas.microsoft.com/sqlserver/2008/DataDebugger/";
private const String DATA_PROFILE_NAMESPACE_ALIAS = "dp";
```

```
private const String DATA_PROFILE_COLUMN_XPATH_PATTERN = "/dp:DataProfile/dp:DataProfileOutput/dp:Profiles" + "/dp:ColumnPatternProfile[dp:Column[@Name='{0}']]";
```

```
private const String REGEX_ELEMENT_XPATH_PATTERN = DATA_PROFILE_COLUMN_XPATH_PATTERN + "/dp:TopRegexPatterns/dp:PatternDistributionItem/dp:RegexText/text()";
```

对象名称和描述在这些常量中定义，以及用于向 SSIS 包开发人员传达组件当前状态的所有消息。

23. 向 ValidateEmail 类添加以下私有变量：

```
private String _dataProfileFileName;
private String _dataProfileColumnName;
private String _emailAddressInputColumnName;
private List<String> _regexPatterns = new List<String>();
```

这些变量允许组件的主要设置一次性设置，并在验证和执行期间根据需要重复使用。

24. 在 ValidateEmail 类定义的底部，添加以下私有函数：

```
private Boolean DataProfileColumnExists(String dataProfileName, String columnName)
{
    Boolean result = true;
    XmlDocument dataProfile = new XmlDocument();
    dataProfile.Load(dataProfileName);
    XmlNamespaceManager dataProfileNSM = new XmlNamespaceManager(dataProfile.NameTable);
```

```
dataProfileNSM.AddNamespace(DATA_PROFILE_NAMESPACE_ALIAS,
DATA_PROFILE_NAMESPACE);

String regexElementXPath =
String.Format(DATA_PROFILE_COLUMN_XPATH_PATTERN, columnName);
XmlNode dataProfileColumn =
dataProfile.SelectSingleNode(regexElementXPath, dataProfileNSM);
if (dataProfileColumn == null)
{
    result = false;
}

return result;
}
```

25. 此函数检查数据配置文件是否包含有关指定列的配置文件信息：

```
private List<String> LoadRegularExpressions(String dataProfileName,
String columnName)
{
    List<String> result = new List<String>();

    if (!String.IsNullOrEmpty(dataProfileName) &&
        !String.IsNullOrEmpty(columnName))
    {
        XmlDocument dataProfile = new XmlDocument();
        dataProfile.Load(dataProfileName);
        XmlNamespaceManager dataProfileNSM = new
XmlNamespaceManager(dataProfile.NameTable);
        dataProfileNSM.AddNamespace(DATA_PROFILE_NAMESPACE_ALIAS,
DATA_PROFILE_NAMESPACE);

        String regexElementXPath =
String.Format(REGEX_ELEMENT_XPATH_PATTERN, columnName);
        foreach (XmlNode regexPatternElement in
dataProfile.SelectNodes(regexElementXPath, dataProfileNSM))
        {
            String regexPattern = regexPatternElement.Value;
            if (!result.Contains(regexPattern))
            {
                result.Add(regexPattern);
            }
        }
    }

    return result;
}
```

