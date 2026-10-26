
- 19. 将以下枚举添加到类定义中：

```csharp
public enum OperationMode
{
    GetFiles,
    PutFiles
}
```

该枚举列出了**安全 FTP 任务**支持的操作模式；其中两种模式使用本示例中的代码实现：`GetFiles` 从指定的 FTP 站点接收文件，`PutFiles` 将文件发送到指定的 FTP 站点。当然，枚举本身并不包含执行这些操作所需的完整编程逻辑。

![](img/07d1225e179d5df9f2b2d26b026386c6_592_0.png)

你可以通过实现 WinSCP 库中提供的其他函数来扩展其范围。关于该库的更多信息，请访问 `https://winscp.net/eng/docs/start`。

- 20. 将以下私有函数添加到 `SecureFTP` 类中：

```csharp
private Session EstablishSession()
{
    Session winScpSession = new Session();

    Protocol ftpProtocol = (Protocol)Enum.Parse(typeof(Protocol),
    this.FtpProtocolName);

    SessionOptions winScpSessionOptions = new SessionOptions
    {
        Protocol = ftpProtocol,
        HostName = this.FtpHostName,
        PortNumber = this.FtpPortNumber,
        UserName = this.FtpUserName,
        Password = this.FtpPassword,
        SshHostKeyFingerprint = this.FtpSshHostKeyFingerprint
    };

    winScpSession.Open(winScpSessionOptions);

    return winScpSession;
}
```

此函数创建 `WinSCP Session` 类的一个实例；它连接到 FTP 服务器，建立适当的安全上下文，并允许对远程站点执行这些操作。

```csharp
Boolean fireAgain = false;

try
{
    // Verify the connection.
    using (Session winScpSession = this.EstablishSession())
    {
        componentEvents.FireInformation(0, TASK_NAME,
            SESSION_OPEN_MESSAGE, String.Empty, 0, ref fireAgain);

        // Verify the remote resources.
        OperationMode operation =
            (OperationMode)Enum.Parse(typeof(OperationMode),
            this.FtpOperationName);
        switch (operation)
        {
            case OperationMode.PutFiles:
                Boolean remoteDirectoryExists =
                    winScpSession.FileExists(this.FtpRemotePath);
                if (!remoteDirectoryExists)
                {
                    componentEvents.FireInformation(0, TASK_NAME,
                        String.Format(REMOTE_DIRECTORY_MISSING_MESSAGE_PATTERN,
                        this.FtpRemotePath), String.Empty, 0, ref fireAgain);
                }
                break;
            case OperationMode.GetFiles:
            default:
                Boolean remoteFileExists =
                    winScpSession.FileExists(this.FtpRemotePath);
                if (!remoteFileExists)
                {
                    componentEvents.FireInformation(0, TASK_NAME,
                        String.Format(REMOTE_FILES_MISSING_MESSAGE_PATTERN,
                        this.FtpRemotePath), String.Empty, 0, ref fireAgain);
                }
                break;
        }
    }

    return DTSExecResult.Success;
}
catch (Exception exc)
{
    String exceptionMessage = exc != null ? exc.Message :
        UNKNOWN_EXCEPTION_MESSAGE;
    componentEvents.FireError(0, TASK_NAME,
        String.Format(EXCEPTION_MESSAGE_PATTERN, exceptionMessage),
        String.Empty, 0);
    return DTSExecResult.Failure;
}
```

每当使用该任务的 SSIS 包被验证或执行时，都会用到 `Validate()` 方法；它将报告任何不正确或缺失的设置，并通知调用方 WinSCP 库返回的任何异常。此方法还会调用在第 21 步中创建的 `ValidateProperties()` 方法。

- 24. 在 `Validate()` 方法定义下方留出更多空间，然后再次输入 override 指令；这次选择要重写的 `Execute()` 方法，并用以下代码替换其默认定义：

```csharp
Boolean fireAgain = false;

try
{
    // Create a new FTP session.
    using (Session winScpSession = this.EstablishSession())
    {
        componentEvents.FireInformation(0, TASK_NAME, SESSION_OPEN_MESSAGE, String.Empty, 0, ref fireAgain);

        // Determine the operation mode.
        OperationMode operation =
            (OperationMode)Enum.Parse(typeof(OperationMode), this.FtpOperationName);
        switch (operation)
        {
            case OperationMode.PutFiles:
                // When uploading files, make sure that the destination directory exists.
                Boolean remoteDirectoryExists =
                    winScpSession.FileExists(this.FtpRemotePath);
                if (!remoteDirectoryExists)
                {
                    winScpSession.CreateDirectory(this.FtpRemotePath);
                    componentEvents.FireInformation(0, TASK_NAME, String.Format(REMOTE_DIRECTORY_CREATED_MESSAGE_PATTERN, this.FtpRemotePath), String.Empty, 0, ref fireAgain);
                }
                winScpSession.PutFiles(this.FtpLocalPath, this.FtpRemotePath, this.FtpRemove);
                break;
            case OperationMode.GetFiles:
            default:
                winScpSession.GetFiles(this.FtpRemotePath, this.FtpLocalPath, this.FtpRemove);
                break;
        }
    }

    return DTSExecResult.Success;
}
catch (Exception exc)
{
    String exceptionMessage = exc == null ?
        UNKNOWN_EXCEPTION_MESSAGE : exc.Message;
    componentEvents.FireError(0, TASK_NAME,
        String.Format(EXCEPTION_MESSAGE_PATTERN, exceptionMessage),
        String.Empty, 0);
    return DTSExecResult.Failure;
}
```

- 25. 保存类文件并生成 SSISCustomTasks 项目。如果正确遵循了上述说明，生成应该成功。如果生成失败，请检查“错误列表”窗格，查看是否发生任何错误，并逐一调查和解决每个错误。

- 26. 在 Visual Studio 工具栏中，选择“解决方案配置”选择框中的 **Release**，如下面的截图所示：

![](img/07d1225e179d5df9f2b2d26b026386c6_597_0.png)

- 27. 再次生成 SSISCustomTasks 项目。相关文件现在已创建在 release 文件夹中，可以部署了。

- 28. 使用 Windows 资源管理器在 `C:\SSIS2016Cookbook\Chapter13\Starter\` 文件夹中找到 `Chapter13_Deploy_CustomTask.bat` 命令文件。

- 29. 以管理员身份运行该命令文件。这会将自定义任务程序集复制到相应的文件夹，并在 Windows GAC、SSDT（设计时）和 SSIS 运行时（运行时）中注册这些程序集；这需要将相关程序集注册到 GAC 中。

- 30. 在 SSDT 中，打开位于 `C:\SSIS2016Cookbook\Chapter13\Starter\AdventureWorksETL\` 文件夹中的 `AdventureWorksETL.sln` 解决方案。

- 31. 在控制流设计器中打开 `SecureFtpTask.dtsx` SSIS 包。

- 32. 检查包属性和变量；此包包含配置你在本示例前面创建的**安全 FTP 任务**所需的大部分信息。

- 33. 确保控制流设计器处于活动状态，并检查 **SSIS 工具箱**。**安全 FTP 任务**应列在**常用**部分。如果找不到该任务，请右键单击 **SSIS 工具箱**，然后在快捷菜单中选择 **刷新工具箱**。如果这样没有帮助，请关闭 SSDT 并重新打开解决方案。如果仍然不行，请关闭 SSDT，并重复步骤 26 到 33。

- 34. 从 **SSIS 工具箱**中，将**安全 FTP 任务**拖到控制流设计器中，并将其名称改为 **下载文件**。

- 35. 双击该任务以打开其编辑器；应弹出以下警告：

![](img/07d1225e179d5df9f2b2d26b026386c6_598_0.png)

你没有为自定义任务设计自定义编辑器；因此，要使用该任务，需要在**属性**窗格中配置其属性。

- 36. 在控制流中选中**安全 FTP 任务**时，请在**属性**窗格中检查其属性。除了基本任务属性之外，你在第 18 步中创建的 `SecureFTP` 类的所有公共成员都应列在**杂项**部分中。

- 37. 找到**表达式**集合，单击其文本框，然后单击最右侧的省略号图标，启动**属性表达式编辑器**。

- 38. 在**属性表达式编辑器**的**属性**列中，选择 `FtpHostName` 属性。

- 39. 单击同一行最右侧的省略号图标，打开**表达式生成器**对话框。

