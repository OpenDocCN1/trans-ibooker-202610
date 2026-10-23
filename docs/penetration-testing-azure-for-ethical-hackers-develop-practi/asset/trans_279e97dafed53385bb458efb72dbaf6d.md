

例如，如果账户在五次尝试失败后锁定，我们通常只会在等待锁定计时器重置前尝试猜三个密码。如有疑问，请务必与负责渗透测试范围的人员合作，以确定合适的密码猜测策略：

![](img/4fa338e84e0a590967a142dd798ba4e2_120_0.png)

*图 3.19 – 暴力破解攻击*

**密码喷洒攻击（password spray attack）** 对多个用户名使用同一密码（见图 3.20）。这里需要考虑的一点是，将密码猜测尝试分散到多个 IP 地址，以避免被某些安全工具检测到（包括微软的 AD 密码保护和身份保护。这些工具的链接可以在本章末尾的*扩展阅读*部分找到）。

可以使用 FireProx 之类的工具，通过 AWS API Gateway 服务发起攻击来轮换源 IP 地址。请记住，FireProx 很可能违反 AWS 可接受使用政策，这可能会给你的 AWS 账户带来问题：

| FireProx 信息 | |
|---|---|
| **创建者** | Mike Felch（推特：@ustayready） |
| **开源或商业** | 开源项目 |
| **GitHub 仓库** | https://github.com/ustayready/fireprox |
| **语言** | Python |

![](img/4fa338e84e0a590967a142dd798ba4e2_121_0.png)

*图 3.20 – 密码喷洒攻击*

既然我们有了猜测密码的方法，接下来让我们了解一些工具，可以用它们来尝试针对我们自己的测试环境猜测密码。

## 介绍 MSOLSpray

MSOLSpray 是一款面向 Microsoft 在线账户（Azure/Office365）的 PowerShell 密码喷洒工具。该工具的优势在于，除了检测有效密码外，它还能提供与账户的多因素认证（MFA）状态相关的有用侦察信息，例如账户是否存在、账户是否被锁定或禁用，或者密码是否已过期。

| MSOLSpray 信息 | |
|---|---|
| **创建者** | Beau Bullock（推特：@dafthack） |
| **开源或商业** | 开源项目 |
| **GitHub 仓库** | https://github.com/dafthack/MSOLSpray |
| **语言** | PowerShell |

虽然还有其他变体已被编写出来（例如 MSOLSpray Python、TREVORspray 等），但在下一个动手练习中，我们将使用最初用 PowerShell 编写的 MSOLSpray 工具。

## 动手练习 – 使用 MSOLSpray 猜测 Azure Active Directory 凭据

在此动手练习中，我们将使用 MSOLSpray 工具执行密码喷洒攻击。以下是我们要完成的任务：

- **任务 1**：从 Azure Cloud Shell 在你的 Azure Active Directory 租户中创建测试用户。
- **任务 2**：使用 Azure 门户为测试用户启用 MFA。
- **任务 3**：在你的渗透测试虚拟机上下载并导入 MSOLSpray。
- **任务 4**：在你的渗透测试虚拟机上创建用户列表。
- **任务 5**：从你的渗透测试虚拟机使用 MSOLSpray 运行密码喷洒攻击。

让我们开始：

1. 打开 Web 浏览器，浏览到 Azure 门户 https://portal.azure.com。使用你的 azureadmin 凭据登录。

2. 在 Azure 门户中，点击右上角的 Cloud Shell 图标：

    ![](img/4fa338e84e0a590967a142dd798ba4e2_122_0.png)

    *图 3.21 – 打开 Azure Cloud Shell*

3. 如果出现提示，请选择 PowerShell：

    ![](img/4fa338e84e0a590967a142dd798ba4e2_122_1.png)

    *图 3.22 – 选择“PowerShell”*

    然后，点击“创建存储”：

    ![](img/4fa338e84e0a590967a142dd798ba4e2_123_0.png)

    *图 3.23 – 为 Cloud Shell 创建云驱动器*

4. 在 Cloud Shell 窗格中的 PowerShell 会话中，运行以下命令下载一个脚本，用于在 Azure AD 中配置测试用户：

    ```powershell
    Invoke-WebRequest -Uri http://bit.ly/az-pentest-create-test-users -OutFile create-test-users.ps1
    ```

    如下所示：

    ![](img/4fa338e84e0a590967a142dd798ba4e2_123_1.png)

    *图 3.24 – 从我们的 GitHub 仓库下载 PowerShell 脚本*

5. 使用以下命令验证脚本是否成功下载。你应该会看到一个名为 create-test-users.ps1 的脚本：

    ```powershell
    Get-ChildItem
    ```

    输出如下所示：

    ![](img/4fa338e84e0a590967a142dd798ba4e2_123_2.png)

    *图 3.25 – 验证 PowerShell 脚本已下载*

6. 使用以下命令执行下载的脚本。当提示输入密码时，输入 myPassword123 并按 Enter。脚本将使用你输入的密码在你的 Azure AD 租户中配置五个新用户：

    ```powershell
    ./create-test-users.ps1
    ```

    如下所示：

    ![](img/4fa338e84e0a590967a142dd798ba4e2_124_0.png)

    *图 3.26 – 为测试用户输入密码*

7. 在脚本输出的末尾，记下用户主体名称和密码。你将在密码喷洒攻击中用到它们：

    ![](img/4fa338e84e0a590967a142dd798ba4e2_124_1.png)

    *图 3.27 – 记下用户主体名称和密码*

8. 关闭 Cloud Shell 窗口：

    ![](img/4fa338e84e0a590967a142dd798ba4e2_124_2.png)

    *图 3.28 – 关闭 Cloud Shell*

9. 在 Azure 门户中，点击左上角的 Hub 菜单图标，然后选择 Azure Active Directory：

    *图 3.29 – 点击“Azure Active Directory”*

10. 在 Azure AD 控制台的“管理”部分，选择“用户”，然后点击“多因素认证”：

    *图 3.30 – 点击“多因素认证”*

11. 这将打开一个新的“多因素认证设置”窗口。选择用户 adaeze 和 juan，然后点击“启用”以启用 MFA：

    ![](img/4fa338e84e0a590967a142dd798ba4e2_126_0.png)

    *图 3.31 – 为“adaeze”和“juan”启用 MFA*

12. 出现提示时，点击“启用多因素认证”，然后点击“关闭”：

    ![](img/4fa338e84e0a590967a142dd798ba4e2_126_1.png)

    *图 3.32 – 启用 MFA*

13. 在你的渗透测试虚拟机上，打开 PowerShell 控制台，切换目录到你的用户目录，并使用以下命令克隆 MSOLSpray GitHub 仓库：

    ```powershell
    cd C:\Users\$env:USERNAME\
    git clone https://github.com/dafthack/MSOLSpray.git
    ```

    如下所示：

    ![](img/4fa338e84e0a590967a142dd798ba4e2_127_0.png)

    *图 3.33 – 克隆 MSOLSpray GitHub 仓库*

14. 使用以下命令导入 MSOLSpray PowerShell 模块：

    ```powershell
    cd .\MSOLSpray\
    Import-Module .\MSOLSpray.ps1
    ```

15. 使用以下命令创建用户名列表：

    ```powershell
    notepad userlist.txt
    ```

    当提示创建新文件时，点击“是”：

    ![](img/4fa338e84e0a590967a142dd798ba4e2_127_1.png)

    *图 3.34 – 创建新的用户列表*

16. 将你之前记下的用户名输出粘贴到记事本中。保存并关闭记事本文档：

    ![](img/4fa338e84e0a590967a142dd798ba4e2_128_0.png)

    *图 3.35 – 粘贴之前脚本创建的用户名*

17. 使用以下命令对用户账户运行 MSOLSpray。指定你之前记下的密码输出。你应该会看到所有账户的成功消息，同时也会看到你启用了 MFA 的账户的输出：

    ```powershell
    Invoke-MSOLSpray -UserList ./userlist.txt -Password myPassword123
    ```

    输出如下所示：

    ![](img/4fa338e84e0a590967a142dd798ba4e2_128_1.png)

    *图 3.36 – 对用户列表执行密码喷洒攻击*

18. 在 InPrivate 或隐身窗口中打开 Azure AD 门户 https://aad.portal.azure.com。使用 Sandra 的凭据登录：

    用户名：sandra@<域名后缀>
    密码：myPassword123

19. 你现在应该能够以 Sandra 的身份访问 Azure AD 门户。此外，你还应该能够以 Sandra 的身份查看 Azure AD 租户中其他用户的信息！（这是默认行为。）

    ![](img/4fa338e84e0a590967a142dd798ba4e2_129_0.png)

    *图 3.37 – 使用 Sandra 被攻破的账户访问 Azure AD 门户*

20. 返回到 Azure Cloud Shell，以 azureadmin 用户身份登录。下载清理脚本以移除之前创建的测试用户，并使用以下命令验证下载：

    ```powershell
    Invoke-WebRequest -Uri http://bit.ly/az-pentest-cleanup-test-users -OutFile cleanup-test-users.ps1
    Get-ChildItem
    ```

    输出如下所示：

    ![](img/4fa338e84e0a590967a142dd798ba4e2_129_1.png)

    *图 3.38 – 下载清理脚本*

