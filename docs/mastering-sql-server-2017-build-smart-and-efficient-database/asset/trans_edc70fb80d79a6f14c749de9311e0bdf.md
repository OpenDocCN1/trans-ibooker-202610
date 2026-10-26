
`Canceled` 用于向 SSIS 执行引擎报告执行已被中断；例如，在执行过程中甚至尚未达到可以被解释为其他三种结果之一的位置时。

通常，在 `Validate()` 方法中只应使用 `Success` 和 `Failure`。

若要部署自定义任务（无论是部署到用于 SSIS 包开发的开发工作站，还是部署到将要使用的环境中），都需要将包含该任务的程序集复制到以下文件夹：

- `%ProgramFiles%\Microsoft SQL Server\130\DTS\Tasks` - 用于程序集的 64 位版本
- `%ProgramFiles(x86)%\Microsoft SQL Server\130\DTS\Tasks` - 用于程序集的 32 位版本

如果程序集与平台无关，则文件需要复制到这两个文件夹中。

程序集及其引用的所有程序集还必须在**全局程序集缓存（GAC）**中注册。要在 SSIS 开发工作站上注册程序集，可以使用 `gacutil.exe` 命令行实用工具（它随 Visual Studio 一起安装）；但在生产服务器上，`gacutil.exe` 可能不可用。您也可以使用适当的 Windows PowerShell 脚本执行注册。

在本节中，您将开发一个自定义任务，将其部署到开发工作站，并在 SSIS 包中使用它。

此自定义任务将使用一个外部库，使您能够在 SSIS 中使用 (SSH FTP) 执行 FTP 任务。

![](img/07d1225e179d5df9f2b2d26b026386c6_585_0.png)

为了方便您使用，本章所需的 C# 代码片段在 `Chapter13.txt` 文件中提供。

## 准备工作

在开始之前，您需要从 `https://winscp.net/` 网站安装 `WinSCP` 类库；WinSCP 是一个免费工具，您可以根据自由软件基金会发布的 GNU 通用公共许可条款（`https://www.gnu.org/licenses/gpl.html`）在自己的解决方案中使用它。

请查看 WinSCP GNU 许可，网址为 https://winscp.net/eng/docs/license。

要在您的工作站上安装和注册外部库，请执行以下步骤：

- 1. 从 https://winscp.net/eng/download.php 下载 **安装包** 5.9.5 版，或 WinSCP 的 .NET **程序集/COM 库**。
- 2. 我们建议您使用安装包，它会将应用程序和必要的程序集安装到本章示例所期望的文件夹中。
- 3. 如果您决定使用安装包，请运行它，然后按照安装向导中的说明完成安装。
- 4. 如果您只想下载程序集，请下载存档文件，然后将其解压到 C:\Program Files (x86)\WinSCP\ 文件夹中。
- 5. 在安装成功完成后，或者将文件放入指定文件夹后，使用 Windows 资源管理器导航到 C:\SSIS2016Cookbook\Chapter13\Scripts 文件夹。
- 6. 找到 Chapter13_GAC_WinSCPnet.bat 命令文件，并在记事本中打开它。
- 7. 检查该文件，然后关闭它。
- 8. 在 Windows 资源管理器中，右键单击命令文件，然后从快捷菜单中选择 **以管理员身份运行...**。
- 9. 在**用户帐户控制**对话框中，单击**确定**以允许执行。
- 10. 执行完成后，按任意键关闭命令提示符窗口。

现在您可以开始设计自定义任务了。

## 操作步骤...

- 1. 启动 Visual Studio 2015 并创建一个新项目。
- 2. 在**新建项目**对话框中，确保选择了 **.NET Framework 4.5.2**，然后在**模板\Visual C#**下选择**类库**模板。使用 `SSISCustomTasks` 作为项目名称，将其放在 `C:\SSIS2016Cookbook\Chapter13\Starter` 文件夹中，勾选**为解决方案创建目录**，并使用 `SSISCustomization` 作为解决方案名称。参考以下截图以验证您的设置：

![](img/07d1225e179d5df9f2b2d26b026386c6_587_0.png)

- 3. 单击“确定”确认配置并创建解决方案。
- 4. 在**解决方案资源管理器**中，右键单击新创建的 `SSISCustomTasks` 项目，然后从快捷菜单中选择**属性**，打开 `SSISCustomTasks` 属性窗格。
- 5. 在**应用程序**页面上，将默认命名空间值更改为 `SSIS2016Cookbook`。
- 6. 在**签名**页面上，勾选**为程序集签名**，然后在**选择强名称密钥文件**选择框中选择**<浏览...>**。
- 7. 在**选择文件**对话框中，导航到 `C:\SSIS2016Cookbook\Chapter13\Scripts` 文件夹，并选择 `SSIS2016Cookbook.snk` 强名称密钥文件，如下面的截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_588_0.png)

- 8. 单击**打开**以确认选择。
- 9. 保存解决方案，然后关闭项目属性窗格。
- 10. 在 `Class1.cs` 设计器窗格中，将类的命名空间更改为 `SSIS2016Cookbook`，并将 `Class1` 名称更改为 `SecureFTP`。
- 11. 在**解决方案资源管理器**中，将 `Class1.cs` 文件名更改为 `SecureFTP.cs`，然后保存解决方案。
- 12. 在**解决方案资源管理器**中，右键单击**引用**，然后从快捷菜单中选择**添加引用...**，打开**引用管理器**对话框。
- 13. 在**程序集 / 扩展**下，勾选 `Microsoft.SqlServer.Dts.Design`、`Microsoft.SqlServer.ManagedDTS` 和 **WinSCPnet** 程序集，如下面的截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_589_0.png)

14. 单击“确定”确认选择。

15. 在 SecureFTP.cs 文件中，在源代码顶部，将现有的所有命名空间引用替换为以下内容：

```
using System;
using Microsoft.SqlServer.Dts.Runtime;
using WinSCP;
```

16. SecureFTP 类必须派生自 Microsoft.SqlServer.Dts.Runtime.Task 基类，并且还必须实现 DtsTaskAttribute：

```
[DtsTaskAttribute(
    Description = "Perform FTP operations securely, by using SSH.",
    DisplayName = "Secure FTP Task"
)]
public class SecureFTP : Task
{
    ...
}
```

Microsoft.SqlServer.Dts.Runtime.Task 基类提供了任何 SSIS 控制流任务所需的功能，DtsTaskAttribute 允许您配置自定义任务的一些主要属性。它们共同使程序集中的类能够被识别为 SSIS 任务，进而可以在 SSIS 包中使用它。

17. 向 SecureFTP 类添加以下私有常量：

```
private const String TASK_NAME = "Secure FTP Task";
private const String FtpProtocolName_MISSING_MESAGE = "FtpProtocolName has not been set.";
private const String FtpHostName_MISSING_MESAGE = "FtpHostName has not been set.";
private const String FtpUserName_MISSING_MESAGE = "FtpUserName has not been set.";
private const String FtpPassword_MISSING_MESAGE = "FtpPassword has not been set.";
private const String FtpSshHostKeyFingerprint_MISSING_MESAGE = "FtpSshHostKeyFingerprint has not been set.";
private const String FtpOperationName_MISSING_MESAGE = "FtpOperationName has not been set.";
private const String FtpLocalPath_MISSING_MESAGE = "FtpLocalPath has not been set.";
```

```
private const String FtpRemotePath_MISSING_MESSAGE = "FtpRemotePath has not been set.";
private const String REMOVE_ENABLED_MESSAGE = "FtpRemove is set to TRUE, which means that the file is going to be removed from the source.";
private const String SESSION_OPEN_MESSAGE = "Session opened succesfully.";
private const String REMOTE_DIRECTORY_MISSING_MESSAGE_PATTERN = "The specified remote [{0}] directory is missing.\r\nIt will be created.";
private const String REMOTE_DIRECTORY_CREATED_MESSAGE_PATTERN = "The specified remote [{0}] directory has been created.";
private const String REMOTE_FILES_MISSING_MESSAGE_PATTERN = "The specified remote file(s) [{0}] cannot be found.";
private const String EXCEPTION_MESSAGE_PATTERN = "An error has occurred:\r\n\r\n{0}";
private const String UNKNOWN_EXCEPTION_MESSAGE = "(No other information available.)";
```

这些常量将由自定义任务的方法使用，以便在配置任务时向 SSIS 包开发人员传达任务状态信息。

18. 创建 SecureFTP 类的以下公共成员：

```
public String FtpProtocolName { get; set; }
public String FtpHostName { get; set; }
public Int32 FtpPortNumber { get; set; }
public String FtpUserName { get; set; }
public String FtpPassword { get; set; }
public String FtpSshHostKeyFingerprint { get; set; }
public String FtpOperationName { get; set; }
public String FtpLocalPath { get; set; }
public String FtpRemotePath { get; set; }
public Boolean FtpRemove { get; set; }
```

SecureFTP 类的这些公共成员将在 SSDT 中作为任务属性访问，并将允许 SSIS 包开发人员配置自定义任务；在 SSDT 中，这些值可以以字面值形式提供，也可以通过表达式提供。
