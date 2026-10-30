

GCP 提供了客户管理 KEK 的能力，让我们能够控制密钥的生成、密钥的轮换以及密钥的过期。密钥仍将存储在 KMS 中，但我们将控制其生命周期。这被称为 CMEK。为了有效组织密钥，Google Cloud KMS 使用密钥环的概念将密钥分组在一起并将继承的权限推送到密钥。

我们可以通过执行多个步骤来创建我们的密钥。让我们开始吧：

1. 通过从 GCP 控制台浏览到 **Security | Key Management**，我们可以创建新的密钥环。密钥环将密钥分组在一起。我们必须做的第一件事是启用 IP，然后选择密钥环位置。我们可以选择一个区域，也可以选择 **global**。我们的决定会影响使用该密钥的应用程序的性能以及创建密钥时可用的选项。在以下示例中，我们选择了 **global**。全局位置是一种特殊的跨区域位置，其数据中心分布在世界各地，并且无法控制所选的确切数据中心。我们只有在应用程序分布在全球、读写操作不频繁且密钥没有地理驻留要求时才应使用 **global**：

## Create key ring

Key rings group keys together to keep them organized. In the next step, you’ll create keys that are in this key ring. Learn more

Project name
qwiklabs-gcp-03-e5a43cc9b75e

Key ring name *
gcp-architect-key-ring

Key ring location *
global

CREATE   CANCEL

Figure 15.23 – Create key ring

一旦我们创建了密钥环，我们将被提示创建密钥。我们可以在密钥环下创建多个密钥，这些密钥将负责加密和解密数据。创建新密钥时，您可以看到我们有设置密钥用途的选项；例如，密钥是否仅用于加密、解密或两者兼有。

> Tip
> 在决定选择哪个位置时，应考虑网络性能。

2. 我们要做的第一个选择是我们想要什么类型的密钥。我们有以下选项。正如我们选择了全局密钥环一样，我们无法选择外部管理的密钥。我们可以从描述中看出，生成的密钥是标准客户管理的加密密钥：

## What type of key do you want to create?

- Generated key
  A standard customer managed encryption key. The key material will be generated for you.
  Learn more
- Imported key
  For importing your key material into GCP. Learn more
- Externally managed key
  The key material will be stored in an external key manager. Learn more

Figure 15.24 – Selecting the type of key

3. 此外，我们还可以选择轮换周期以及密钥有效期的开始时间。创建密钥后，我们可以通过 GCP 控制台管理轮换周期、禁用和重新启用密钥，或完全删除密钥。以下屏幕截图显示了我们创建新密钥的过程：

![](img/de2c97ab017f36b3750a5bade111edd7_496_0.png)

Figure 15.25 – Create key

KMS 密钥结构的一个重要方面是密钥版本。每个密钥可以有多个版本，按顺序编号，从 1 开始。我们可能使用相同的密钥但使用不同的密钥版本来加密文件。Cloud KMS 会自动识别用于加密的版本，并在版本仍启用时使用它来解密文件。如果版本已移动到禁用、销毁或计划销毁的状态，我们将无法解密文件。

4. 现在我们已经创建了密钥，让我们使用它。例如，如果我们想在 GCP 中创建一个新磁盘，我们在加密设置下有几个选项。要使用我们新创建的密钥，我们应选择 Customer-managed key 并在提示时从下拉菜单中选择我们的密钥，如以下屏幕截图所示：

![](img/de2c97ab017f36b3750a5bade111edd7_497_0.png)

Figure 15.26 – Using Customer-managed encryption key (CMEK)

另一个可用的选项是 CSEK，其中密钥不存储在 KMS 中，Google 不管理密钥。CSEK 允许我们提供自己的 AES-256 密钥。如果我们提供此密钥，Cloud Storage 不会永久将其存储在 Google 的服务器上，也不会以任何方式管理我们的密钥。一旦我们为 Cloud Storage 操作提供了密钥，该密钥就会从 Google 的内存中清除。这意味着每次创建或使用存储资源时都必须提供该密钥。客户将承担全部责任，这意味着如果密钥丢失，则无法解密您的数据。在这里，我们将选择 Customer-supplied encryption key (CSEK) 选项并输入相关密钥。

现在，让我们开始了解一些行业法规。

## Industry regulations

同样重要的是要了解，安全不仅仅是防火墙规则或加密。Google 需要遵守全球法规和第三方认证。GCP 遵守的法规示例可以在其网页 https://cloud.google.com/security/compliance/#/ 上找到。建议您查看此页面以熟悉应满足的各种标准。其中一些将是众所周知的；例如，来自金融行业的法规，如 PCI DSS 或 ISO 27017，负责云供应商和客户之间的交互。

在本节中，我们将介绍 PCI 合规性。

## PCI compliance

许多组织处理金融交易，Google 必须竭尽全力保护其服务器上的信息。PCI 的一个例子可以将我们之前学到的内容与现实生活中的例子联系起来。如果需要设置特定的支付处理环境，Google 可以帮助客户实现这一目标。该架构的核心是我们本章学到的内容。为了保护环境，我们应使用 Resource Manager 创建单独的项目以隔离我们的游戏和 PCI 项目。我们可以利用 Cloud IAM 并将权限应用于这些单独的项目。记住最小权限原则！我们还可以使用防火墙规则来限制入站流量以保护环境。我们希望公众能够使用我们的支付页面，因此我们需要 HTTPS 流量由 HTTP(S) 负载均衡器保护，并且任何额外的支付处理应用程序可能需要与第三方进行双向访问。请查看文章 https://cloud.google.com/solutions/pci-dss-compliance-in-gcp 以更深入地了解 GCP 如何处理 PCI DSS 要求。

在下一节中，我们将回顾数据丢失防护。

## Data loss prevention (DLP)

删除 **Personal Identifiable Information (PII)** 也是行业内的一个关注点。Cloud DLP 为我们提供了一个强大的数据和去识别化平台。DLP 使用信息类型，也称为 infoTypes，来定义要扫描的内容。这些 infoTypes 是敏感数据类型，例如姓名、电子邮件地址、信用卡号或识别号码。Cloud DLP 如何帮助我们的一个例子是，我们可以自动从可能存储在 GCP 存储库中的此类敏感数据中进行涂黑。

我们还应注意，遵守 **European Union General Data Protection Regulation (GDPR)** 是 Google 从安全角度的首要任务之一，并且这些内置的 infoType 检测器提供 GDPR 合规性。

## Penetration testing in GCP

值得注意的是，如果您需要对 GCP 基础设施进行渗透测试，您不需要 Google 的许可，但必须遵守可接受使用政策，以确保测试仅针对您的项目。有趣的是，Google 提供了一个激励计划，如果发现错误或漏洞，奖励范围从 $100 到 $31,337！

## CI/CD security overview

随着 DevOps 越来越受欢迎，在构建管道、镜像和交付模型时考虑安全非常重要。Google Cloud 在构建 CI/CD 管道时提供了一些最佳实践。然而，它们像许多最佳实践一样是主观的，可能不适用于您的架构。尽管如此，考虑以下事项仍然很重要：

- **Branching model**: 重要的是要考虑对代码的更改将如何影响生产环境，因此可能需要功能分支来在对生产代码库进行任何合并请求之前测试更改。
- **Container image vulnerabilities**: 在创建要在管道中使用的镜像时，在镜像上传到 Artifact 或 Container Registry 时扫描漏洞也很重要。在这种情况下，可以使用 Cloud Build 在镜像构建后扫描镜像，然后阻止其上传到 Artifact Registry。这称为按需扫描。
- **Filesystem security**: 即使在容器中，防止潜在攻击者安装其工具也很重要。我们应避免在容器内以 root 身份运行，并在只读模式下启动容器。
- 容器的大小也应考虑，不仅从性能角度，而且从安全角度。较小的容器意味着更小的攻击面。许多较大的容器显示与已安装但与应用程序无关的程序相关的漏洞。

现在，让我们来看 Identity-Aware Proxy。

