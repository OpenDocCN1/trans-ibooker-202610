

```
}
else
{
    IDTSInputColumn100 inputColumn =
ComponentMetaData.InputCollection[INPUT_NAME].InputColumnCollection
[_emailAddressInputColumnName];
    if (inputColumn == null)
    {
        ComponentMetaData.FireError(0, ComponentMetaData.Name,
String.Format(INPUT_COLUMN_NOT_FOUND_MESSAGE_PATTERN,
inputColumn.Name), String.Empty, 0, out isCanceled);
        return DTSValidationStatus.VS_ISBROKEN;
    }
    else
    {
        ComponentMetaData.FireInformation(0,
ComponentMetaData.Name,
String.Format(INPUT_COLUMN_FOUND_MESSAGE_PATTERN,
inputColumn.Name), String.Empty, 0, ref fireAgain);

        if (inputColumn.DataType != DataType.DT_NTEXT &&
            inputColumn.DataType != DataType.DT_TEXT &&
            inputColumn.DataType != DataType.DT_STR &&
            inputColumn.DataType != DataType.DT_WSTR)
        {
            ComponentMetaData.FireError(0, ComponentMetaData.Name,
String.Format(INPUT_COLUMN_DATATYPE_NOT_SUPPORTED_MESSAGE_PATTERN,
inputColumn.DataType.ToString(), inputColumn.Name), String.Empty,
0, out isCanceled);
            return DTSValidationStatus.VS_ISBROKEN;
        }
    }
}
return base.Validate();
```

每当包在 SSDT 中加载时、SSIS 开发人员在执行开始时确认组件配置时，以及显式验证已部署的包时，都会对组件进行验证。

### 30. 为了指导 SSIS 包开发人员，并防止他们无意中破坏组件的配置，请在 ValidateEmail 类定义中添加以下重写：

```
public override IDTSInput100 InsertInput(DTSInsertPlacement
insertPlacement, Int32 inputID)
{
    // Only one input is supported.
```
```
throw new NotSupportedException(TOO_MANY_INPUTS_MESSAGE);
}
```

此方法将阻止 SSIS 开发人员创建任何额外的输入：

```
public override IDTSOutput100 InsertOutput(DTSInsertPlacement insertPlacement, Int32 outputID)
{
    // Only one output is supported.
    throw new NotSupportedException(TOO_MANY_OUTPUTS_MESSAGE);
}
```

此方法将阻止 SSIS 开发人员创建任何额外的输出：

```
public override IDTSOutputColumn100 InsertOutputColumnAt(Int32 outputID, Int32 outputColumnIndex, String name, String description)
{
    // No additional Output Columns can be added.
    throw new NotSupportedException(USER_DEFINED_COLUMNS_NOT_SUPPORTED);
}
```

此方法将阻止 SSIS 开发人员向默认输出添加任何额外的列：

```
public override void DeleteOutput(Int32 outputID)
{
    // The built-in output cannot be removed.
    Boolean isInternal =
    (Boolean)(ComponentMetaData.OutputCollection.GetObjectByID(outputID).CustomPropertyCollection[IS_INTERNAL_OBJECT_PROPERTY_NAME].Value);
    if (isInternal)
    {
        throw new InvalidOperationException(DEFAULT_OUTPUT_MUST_EXIST_MESSAGE);
    }
    else
    {
        base.DeleteOutput(outputID);
    }
}
```

此方法将阻止 SSIS 开发人员删除默认输出；它允许他们删除其他输出，但这只是理论上的，因为添加输出已经被前面的 `InsertOutput()` 方法重写所阻止：

```
public override void DeleteOutputColumn(Int32 outputID, Int32 outputColumnID)
{
    // Built-in output columns cannot be removed.
    Boolean isInternal =
    (Boolean)(ComponentMetaData.OutputCollection.GetObjectByID(outputID).OutputColumnCollection.GetObjectByID(outputColumnID).CustomPropertyCollection[IS_INTERNAL_OBJECT_PROPERTY_NAME].Value);
    if (isInternal)
    {
        throw new InvalidOperationException(DEFAULT_COLUMNS_MUST_EXIST);
    }
    else
    {
        base.DeleteOutputColumn(outputID, outputColumnID);
    }
}
```

这最后一个重写将阻止 SSIS 开发人员删除内置输出列。理论上，它允许他们删除用户自定义列；然而，重写的 `InsertOutputColumnAt()` 方法从根本上阻止了这些列的添加。

- 31. 使用与步骤 26 和 27 相同的技术，重写 `PreExecute()` 基类方法，并使用以下代码作为其定义：

```
base.PreExecute();

this.ResolveComponentCustomProperties();
```

处理管道数据所需的所有设置都在 `PreExecute()` 方法中确定。

- 32. 接下来，重写 `ProcessInput()` 方法，并用以下命令替换其默认定义：

```
IDTSInput100 input =
ComponentMetaData.InputCollection.GetObjectByID(inputID);
Int32 emailAddressInputColumnId =
input.InputColumnCollection[_emailAddressInputColumnName].ID;
IDTSInputColumn100 emailAddressInputColumn =
input.InputColumnCollection.GetObjectByID(emailAddressInputColumnId);
Int32 emailAddressInputColumnIndex =
input.InputColumnCollection.GetObjectIndexByID(emailAddressInputColumnId);

IDTSOutput100 output =
ComponentMetaData.OutputCollection[OUTPUT_NAME];
Int32 isValidColumnId =
output.OutputColumnCollection[IS_VALID_COLUMN_NAME].ID;
IDTSOutputColumn100 isValidColumn =
output.OutputColumnCollection.GetObjectByID(isValidColumnId);
Int32 isValidColumnIndex =
BufferManager.FindColumnByLineageID(input.Buffer,
isValidColumn.LineageID);

while (buffer.NextRow())
{
    String emailAddress;
    switch (emailAddressInputColumn.DataType)
    {
        case DataType.DT_NTEXT:
            emailAddress =
Encoding.Unicode.GetString(buffer.GetBlobData(emailAddressInputColumnIndex, 0,
(Int32)buffer.GetBlobLength(emailAddressInputColumnIndex)));
            break;
        case DataType.DT_TEXT:
            emailAddress =
Encoding.GetEncoding(emailAddressInputColumn.CodePage).GetString(buffer.GetBlobData(emailAddressInputColumnIndex, 0,
emailAddressInputColumn.Length));
            break;
```

```
```csharp
default:
    emailAddress =
        buffer.GetString(emailAddressInputColumnIndex);
        break;
}

buffer.SetBoolean(isValidColumnIndex,
    this.IsValidEmail(emailAddress));
}
```
```

列值验证在 `ProcessInput()` 方法中执行。根据输入列数据类型从指定列检索数据——`DT_TEXT` 和 `DT_NTEXT` 大型对象（LOB）数据类型需要特殊处理，而 `DT_STR` 和 `DT_WSTR` 中的数据可以简单地使用 `GetString()` 方法读取。

- 33. 在重写的 `PostExecute()` 方法中，使用以下命令：
- 
-       `_regexPatterns.Clear();`
- 
-       `base.PostExecute();`
- 
- 处理完行后，`PostExecute()` 方法释放资源；它清除正则表达式集合，因为不再需要它。

- 34. 保存解决方案，并从**生成**菜单中选择**生成 SSISCustomComponents**命令来生成项目——首先使用**Debug**，然后再次使用**Release**解决方案配置。如果你正确遵循了前面的说明，项目应能成功生成。如果出现任何错误，请检查**错误列表**窗格中的消息并做出适当更正。

- 35. 使用 Windows 资源管理器导航到 `C:\SSIS2016Cookbook\Chapter13\Starter` 文件夹，找到 `Chapter10_Deploy_CustomComponent.bat` 命令文件。

- 36. 以管理员身份执行该命令文件，观察**命令提示符**窗口中返回的消息，最后按任意键完成部署。

- 37. 在 SSDT 中，打开位于 `C:\SSIS2016Cookbook\Chapter13\Starter\AdventureWorksETL\` 文件夹中的 `AdventureWorksETL.sln` 解决方案。

- 38. 在控制流中打开 `RegExValidation.dtsx` 包；该包基于你在第 11 章*释放 SSIS 脚本任务和组件的威力*中的*在脚本组件中使用正则表达式验证数据*所创建的类似同名包。

- 39. 在数据流设计器中打开**验证人员数据**任务。

- 40. 从 SSIS 工具箱中，将**验证电子邮件**转换拖到数据流设计器中；它应位于**常用**部分。如果 SSIS 工具箱中没有**验证电子邮件**转换，请右键单击工具箱，然后从快捷菜单中选择**刷新工具箱**。如果这没有帮助，请关闭 SSDT，然后重新打开 `AdventureWorksETL.sln` 解决方案。如果仍然无法解决问题，请返回打开 `SSISCustomization.sln` 的 Visual Studio 实例，并重复步骤 32 到 34 重新部署程序集。

- 41. 将常规数据路径从**人员数据**源组件连接到**验证电子邮件**转换。

- 42. 双击**验证电子邮件**转换，打开 **Validate Email 的高级编辑器**。

- 43. 在**组件属性**页面上，使用以下设置配置组件的自定义属性：

| 属性 | 值 |
|---|---|
| Data Profile Column Name | `EmailAddress` |
| Data Profile File Name | `$Package::DataProfileName` |

