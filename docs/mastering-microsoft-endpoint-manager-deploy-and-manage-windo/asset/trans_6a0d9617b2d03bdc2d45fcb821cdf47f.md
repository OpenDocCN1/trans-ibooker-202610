

- 4. 点击 **Search Universal Print for printers**。你的已分配云打印机应会自动弹出：

![](img/a3e9f26159d3739e434fe676b2987c1d_569_0.png)

图 15.23 – Printers & scanners – Windows 10

- 5. 点击 **Add device** 将基于云的打印机添加到你的虚拟或物理桌面：

![](img/a3e9f26159d3739e434fe676b2987c1d_570_0.png)

图 15.24 – Add device

- 6. 打印机现已添加并准备测试：

图 15.25 – Printers & scanners – printer added

- 7. 打开基于云的打印机并点击 Manage。

- 8. 点击 Print a test page。测试页已发送到打印机。你可以打开打印队列查看是否有反应：

图 15.26 – Printers & scanners – Print a test page

- 9. 测试打印作业已发送到打印机。

- 10. 如果一切顺利，打印作业也应可在 Universal Print 管理门户中看到并列出。你可以在 Universal Print 门户中点击打印机，然后点击 Jobs 找到作业。你应能看到状态为 Completed 的作业。做得好：

![](img/a3e9f26159d3739e434fe676b2987c1d_572_0.png)

图 15.27 – Universal Print – print jobs

在下一节中，你将学习如何通过 Microsoft Endpoint Manager 发布打印机。

## Assigning and deploying cloud printers with Microsoft Endpoint Manager

在上一节中，你了解了分配打印机的所有基础知识以及手动流程。此流程也可以通过 Microsoft Endpoint Manager 执行，如前所述，这是一种更适合企业的部署方式，例如将全球数百台打印机分配给你的用户：

- 1. 首先，通过此链接下载 Universal Print 部署工具：https://www.microsoft.com/en-us/download/details.aspx?id=101453。
- 2. 准备好用于部署的 CSV 列表，因为 Microsoft Endpoint Manager  enrolment 将使用 CSV 文件作为源。你可以在工具中的 SamplePolicy.zip 中找到 printers.csv 文件。
- 3. 在 CSV 文件中按以下顺序添加所有 Universal Print 打印机共享名称和共享 ID 以及打印机是否应设为默认打印机，SharedId、SharedName、IsDefault：

![](img/a3e9f26159d3739e434fe676b2987c1d_573_0.png)

图 15.28 – Universal Print – Microsoft Endpoint Manager CSV config file

- 4. 通过 Microsoft Endpoint Manager 查找你想要添加的打印机共享名称和共享 ID：

![](img/a3e9f26159d3739e434fe676b2987c1d_573_1.png)

图 15.29 – Universal Print – printer overview

- 5. 生成包含新 CSV 文件的自定义 Win32 应用程序包，其中包含正确的打印机共享名称和 ID。你已在 *Chapter 8, Application Management and Delivery* 中学习过创建包的方法。
- 6. 下载 Microsoft Win32 Content Prep Tool。
- 7. 解压该工具，例如到 C:\ 驱动器上（你可以选择任意文件夹），来源为 https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool：

![](img/a3e9f26159d3739e434fe676b2987c1d_574_0.png)

图 15.30 – Intune – Win32 Content Prep Tool

- 8. 打开 PowerShell 并更改目录到 Intune Prep Tool 位置，例如 C:\Microsoft-Win32-Content-Prep-Tool-master：

![](img/a3e9f26159d3739e434fe676b2987c1d_574_1.png)

图 15.31 – PowerShell

- 9. 运行 .\IntuneWinAppUtil.exe 并填写以下要求：
  - **Source folder**：包含打印机列表 (`printers.csv`) 和 `InstallPolicy.cmd` 文件所在的文件夹。
  - **Setup file**：`InstallPolicy.cmd` 文件的路径（或将用于在用户设备上复制 `printers.csv` 文件的任何其他脚本）。
  - **Output folder**：你希望生成的 `intunewin` 包文件存储的文件夹。
  - **Do you want to specify catalog folder (Y/N)**：输入 N。

![](img/a3e9f26159d3739e434fe676b2987c1d_575_0.png)

图 15.32 – PowerShell

- 10. .intunewin 包已准备好在 Microsoft Endpoint Manager 中使用：

![](img/a3e9f26159d3739e434fe676b2987c1d_575_1.png)

图 15.33 – The .intunewin package ready to use

- 11. 通过 http://endpoint.microsoft.com/ 打开 Microsoft Endpoint Manager 控制台。然后打开 Apps 菜单并点击 Add：

图 15.34 – Intune – Win32 configuration

- 12. 选择 Windows app (Win32)：

图 15.35 – Select app type

- 13. 点击 **Select app package file** 并浏览到 `.intunewin` 文件输出 – 例如 `InstallPolicy.intunewin` 在 `C:\Microsoft-Win32-Content-Prep-Tool-master` 文件夹中：

![](img/a3e9f26159d3739e434fe676b2987c1d_577_0.png)

图 15.36 – App package file

- 14. 为应用指定自定义名称，例如 `Install HP Printer`，如下例所示：

![](img/a3e9f26159d3739e434fe676b2987c1d_577_1.png)

图 15.37 – Add app

- 15. 更改 **Install command** 和 **Uninstall command** 字段中的后续行。确保它是一个 **User** 安装的应用：
  - InstallPolicy.cmd user install
  - InstallPolicy.cmd user uninstall：

![](img/a3e9f26159d3739e434fe676b2987c1d_578_0.png)

图 15.38 – Specify the commands to install and uninstall the app

- 16. 将截图中的要求更改为基线操作系统版本并点击 **Next**。基线定义了接收打印机分配包所需的最低 Windows 版本：

![](img/a3e9f26159d3739e434fe676b2987c1d_579_0.png)

图 15.39 – Intune – Win32 configuration

- 17. 根据包含要添加打印机的 CSV 文件填写检测规则。在 **Path** 字段中添加以下位置，并在 **File or folder** 字段中输入 `printers.csv`：`%AppData%\UniversalPrintPrinterProvisioning\Configuration`。

- 18. 确保 Detection method 设置为 File or folder exists。点击 OK：

![](img/a3e9f26159d3739e434fe676b2987c1d_580_0.png)

图 15.40 – Detection rule

- 19. 在 Assignments 菜单中按部门过滤打印机列表，例如按 AD 组：

![](img/a3e9f26159d3739e434fe676b2987c1d_580_1.png)

图 15.41 – Filter the list of printers

- 20. 通过点击 Next 然后 Create 确认设置：

![](img/a3e9f26159d3739e434fe676b2987c1d_581_0.png)

图 15.42 – Confirm the settings

- 21. 你的打印机随后将被推送到你的物理和云桌面：

![](img/a3e9f26159d3739e434fe676b2987c1d_581_1.png)

图 15.43 – Intune – Win32 configuration

你现在已完成 Universal Print 的配置，并成为云打印的真正专家。这将使你作为 IT 专业人员的生活变得轻松许多，同时与传统打印环境相比维护打印机也更简单。

## Summary

在本章中，你了解了名为 Universal Print 的新 Microsoft 365 服务，如何配置该服务，以及如何通过 Microsoft Endpoint Manager 将打印机发布到你的终端。

在下一章，我们将讨论你可以用 Microsoft Endpoint Manager 管理的其他受管设备。

## Questions

- 1. 为了使用 Universal Print，防火墙中需要开放哪个端口？
  - A. TCP - 443
  - B. TCP - 445
- 2. Universal Print 的打印作业是否在云中加密发送？
  - A. Yes
  - B. No
- 3. Universal Print 是否支持零信任？
  - A. Yes, all traffic goes over the internet over SSL.
  - B. No.

## Answers

- 1. (A)
- 2. (A)
- 3. (A)

## Further reading

如果你想在阅读本章后了解更多关于 Universal Print 的信息，请使用以下免费在线资源：

- Universal Print – Tech Community via aka.ms/upcommunity: https://techcommunity.microsoft.com/t5/universal-print/ct-p/UniversalPrint
- Universal Print – Get started via aka.ms/updocs: https://docs.microsoft.com/en-us/universal-print/fundamentals/universal-print-getting-started

## Section 4: Tips and Tricks from the Field

向作者学习他们在实地最常见的部署和网络相关错误，并学习如何应对和解决它们。

本书的这一部分包含以下章节：

- Chapter 16, Troubleshooting Microsoft Endpoint Manager
- Chapter 17, Troubleshooting Windows 365
- Chapter 18, Community Help

