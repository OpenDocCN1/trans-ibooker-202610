

## 存储账户

存储账户是 Azure 在云中处理数据存储的方式。这些账户由以下子服务组成：

- **Blob 服务：** 通用 HTTP/HTTPS 文件托管
- **Files 服务：** 附加数据存储（SMB/NFS）
- **Table 服务：** NoSQL 半结构化数据集
- **Queue 服务：** HTTP/HTTPS 消息存储

根据之前的子域枚举任务输出，我们知道 azurepentesting 基础词产生了一个名为 azurepentesting 的存储账户。

我们还可以看到以下子域的 DNS 记录：

- **Blob 服务：** azurepentesting.blob.core.windows.net
- **Files 服务：** azurepentesting.file.core.windows.net
- **Table 服务：** azurepentesting.table.core.windows.net
- **Queue 服务：** azurepentesting.queue.core.windows.net

默认情况下，在创建存储账户时会分配这四个子域。这并不一定意味着 Azure 客户将这四项服务全部用于数据存储，因此我们需要确定它们是否包含任何匿名可用的数据。

Files、Table 和 Queue 服务访问时始终需要一定程度的身份验证（本章中我们不会关注它们）。相反，我们将关注 Blob 服务，该服务允许管理员/开发人员配置文件的匿名访问。

## Blob 存储账户权限概述

存储账户中的 Blob 服务由 **容器** 组成。这些容器本质上是可用于存储对象/数据的目录。您可以通过 HTTP/HTTPS 端点访问这些对象。URL 地址是存储账户名称、容器和对象名称的组合。

如以下图所示，如果存储账户名称为 azurepentesting，容器名称为 public，容器内的文件为 README.txt，则对象的 URL 将如下所示：

```
https://azurepentesting.blob.core.windows.net/public/README.txt.
```

对于更注重视觉的学习者，这里是该结构的示意图：

![](img/4fa338e84e0a590967a142dd798ba4e2_110_0.png)

图 3.11 – Azure Blob 服务端点 URL

一个存储账户可以有多个容器，每个容器的公共访问级别配置可能不同。公共访问配置选项如下：

- **Private**：不允许匿名访问。需要经过身份验证才能访问。
- **Blob**：允许匿名访问已知的对象 URL。
- **Container**：允许匿名列出并访问容器文件。

虽然 **Blob** 容器权限可能会泄露敏感数据，但攻击者需要知道受影响文件的完整文件名才能访问它们。在应用程序以可预测的命名格式（如 Report-123.pdf、Report-124.pdf 等）以编程方式创建文件的情况下，攻击者在最初识别出预期用户文件后，可能能够推断出存储账户中其他文件的名称。这种类型的文件名枚举通常被称为 **不安全直接对象引用**，简称为 **IDOR**。PortSwigger 在其网站上对此问题有很好的概述：https://portswigger.net/web-security/access-control/idor。

攻击者还可以使用 Gobuster 等工具执行容器和对象名称猜测攻击，以查看是否返回对象。成功率很低，但仍有可能。

最令人担忧的公共访问级别是 **container**，因为它允许任何人列出所有可用文件并访问它们。这意味着知道存储账户和容器的攻击者将能够枚举所有可用文件。

利用我们之前的示例存储账户 URL，我们知道存储账户名称为 azurepentesting，容器为 public。通过将 ?restype=container&comp=list 参数附加到容器 URL（https://azurepentesting.blob.core.windows.net/public/?restype=container&comp=list），我们可以列出该容器中存储的所有对象。这将返回有关可用文件的 XML 数据，我们随后可以使用它来访问对象！

```
<EnumerationResults ContainerName="https://azurepentesting.blob.core.windows.net/public/">
  <Blobs>
    <Blob>
      <Name>README.txt</Name>
      <Url>https://azurepentesting.blob.core.windows.net/public/README.txt</Url>
      <Properties>
        <Last-Modified>Mon, 21 Dec 2020 00:47:51 GMT</Last-Modified>
        <Etag>0x8DBA54A0BCC7C5C</Etag>
        <Content-Length>123</Content-Length>
        <Content-Type>text/plain</Content-Type>
        <Content-Encoding/>
        <Content-Language/>
        <Content-MD5>P4Xiwn/SRng174UNmV1J2A==</Content-MD5>
        <Cache-Control/>
        <BlobType>BlockBlob</BlobType>
        <LeaseStatus>unlocked</LeaseStatus>
      </Properties>
    </Blob>
  </Blobs>
  <NextMarker/>
</EnumerationResults>
```

图 3.12 – Azure Blob 服务端点 URL

从未经过身份验证的角度来看，如果我们立即不知道存储账户中的任何容器，我们可以做什么？我们可以使用自动化工具来猜测容器名称，检查它们是否允许容器权限，然后从那里列出可用文件。

## 实践练习 – 使用 MicroBurst 识别配置错误的 blob 容器

在本练习中，我们将使用 MicroBurst 中的 Invoke-EnumerateAzureBlobs 脚本来访问配置错误的 Azure Blob 存储服务中的数据。以下是本练习中我们将完成的任务：

- **任务 1：** 使用 azurepentesting 基础名称和内置排列列表，使用 Invoke-EnumerateAzureBlobs MicroBurst 模块匿名枚举存储账户容器。
- **任务 2：** 定义自定义容器列表并使用它来枚举更多容器。

让我们开始：

1. 在你的渗透测试 VM 中，以管理员身份打开 PowerShell 控制台。
2. 将目录路径更改为 MicroBurst 目录并使用以下命令导入 PowerShell 函数：

```
cd .\MicroBurst\
```

```
Import-Module .\MicroBurst.psml
```

以下是输出示例：

![](img/4fa338e84e0a590967a142dd798ba4e2_112_0.png)

图 3.13 – 导入 MicroBurst PowerShell 模块

3. 使用 azurepentesting 作为基础词，使用 Invoke-EnumerateAzureBlobs 函数来识别具有容器访问级别配置的公共存储账户容器：

```
Invoke-EnumerateAzureBlobs -Base azurepentesting
```

4. 你可以看到 MicroBurst 已识别出两个容器（public 和 private）以及这些容器中的对象。

以下是输出示例：

![](img/4fa338e84e0a590967a142dd798ba4e2_113_0.png)

图 3.14 – 使用 MicroBurst 查找公共 blob 容器

5. 我们随后可以直接使用以下命令下载对象的內容：

```
Invoke-WebRequest -Uri "https://azurepentesting.blob.core.windows.net/public/README.txt" -OutFile "README.txt"
Invoke-WebRequest -Uri "https://azurepentesting.blob.core.windows.net/private/credentials.txt" -OutFile "credentials.txt"
```

以下是输出示例：

![](img/4fa338e84e0a590967a142dd798ba4e2_113_1.png)

图 3.15 – 使用 PowerShell 下载对象

6. 使用以下命令读取其中一个下载文件的内容。这将在记事本中打开文件：

```
notepad .\credentials.txt
```

![](img/4fa338e84e0a590967a142dd798ba4e2_114_0.png)

图 3.16 – 访问下载的文件

7. 在步骤 3 中使用的命令中，MicroBurst 使用 private 和 public 等常见容器名称枚举存储账户。除非在命令中另有指定，否则用于容器名称猜测的名称将来自 MicroBurst 仓库中的 permutations.txt 文件。MicroBurst 用于猜测的完整名称列表可通过以下命令查看：

```
Get-Content .\Misc\permutations.txt
```

8. MicroBurst 还允许我们使用 Folders 参数指定自定义容器名称。使用以下命令创建自定义容器名称列表。提示创建新文件时单击 Yes：

```
notepad customcontainer.txt
```

