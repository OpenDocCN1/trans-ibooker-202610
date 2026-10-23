

AAD Connect 是它配置 Azure，以便具有本地 AD 的用户可以访问 Azure 资源。

## FIGURE 2.11 Azure AD Connect wizard

![](img/95043b3df926bcdadadb082b49262037_91_0.png)

除了支持配置三种登录方案（密码哈希同步、直通身份验证和 ADFS，后续会讨论）之外，AAD Connect 还提供同步帮助。同步支持会将本地 AD 中存在的目录对象（如用户、设备和组）创建到云中。在 Azure 门户中，导航到 Azure Active Directory 刀片并单击 Azure AAD Connect，即可查看 Azure AAD Connect 进程的同步状态。图 2.12 表示成功同步前后的外观。

要完成 Azure AAD Connect 同步，必须使用用于同步的账户是存在于 Azure Active Directory 中的全局管理员。这可能并非如此。最初，如图 2.13 所示，我用来创建订阅的用户其来源等于 Microsoft 账户，而另一个用户的来源等于 Azure Active Directory。

## FIGURE 2.12 Azure AD Connect status

![](img/95043b3df926bcdadadb082b49262037_92_0.png)

## FIGURE 2.13 AAD versus Microsoft accounts in the Azure Portal

![](img/95043b3df926bcdadadb082b49262037_92_1.png)

运行 AAD Connect 时必须使用来源为 Azure Active Directory 的账户，并且账户角色必须是全局管理员。

## Connect Health

想象一下，当你同步两个或多个数据存储库时，其中一个或多个可能会出现不同步的情况，即一个存储库中的更新、插入或删除不会立即反映在另一个存储库中，也不会在稍后某个时间点反映，或根本不会反映。瞬时问题或更长时间的停机都可能对数据的连贯性产生一定影响。这不仅适用于 Active Directory，也适用于任何数据存储库。

Azure AAD Connect Health 是一项监控服务，它取代了旧工具如 DirSync 和 Azure AD Sync，并且是一个服务代理，可帮助监控以下等功能的运行状况：

- 与 Azure Active Directory 的同步错误（缺失、过时或重复）。
- 识别尝试 ADFS 登录的恶意行为者 IP。
- 监控并提醒 Azure Active Directory 域服务（后续会讨论）的运行状况问题。
- 基于错误类型配置警报。

Azure AAD Connect Health 可通过门户进行配置，方法是单击 Azure AAD Connect Health 链接。参见图 2.14。

## FIGURE 2.14 Azure AD Connect Health

![](img/95043b3df926bcdadadb082b49262037_93_0.png)

## Directory Objects

目录对象包括用户、设备和组等项目。请记住，Azure Active Directory 是一个身份提供者或权威机构，是用户身份进行身份验证的位置。身份验证后，应用程序可以利用身份所在的组来授权访问。因此，免费版中 500,000 个对象的限制对于大多数客户来说已经足够。然而，该版本缺少在云中运行安全工作负载所需的许多其他企业功能。比较 Azure Active Directory 功能与本地 Active Directory 部署时，特别是对于服务账户和机器账户，某些差异可能并不明显。例如，如果不实施并配置许多其他 Azure 产品和功能，就无法将 Azure VM 添加到 Azure Active Directory。

## Single Sign-On

如上一章所述，为执行工作所需的所有应用程序都提供用户 ID、密码和配置文件并不是最佳做法。因此，创建了一个名为 Active Directory 的全球内部网功能。该实体包含公司所有用户的身份和组分配。单点登录后（例如登录连接到内部网的工作站），该内部网中的任何其他应用程序都可以在不再次询问的情况下重用这些凭据。同样适用于基于云的资源，这意味着任何使用 AAD 进行身份验证的基于云的应用程序，如果用户已在企业网络中通过身份验证，则不会再次请求凭据。

在 AAD 中实施 SSO 时，有三种值得考虑的混合身份登录方法。

**Password Hash Synchronization** 在此，AAD Connect 将用户密码的哈希的哈希从本地同步到 AAD。然后用户使用与连接到本地应用程序时相同的密码登录 Azure 服务。此方法所需的工作量最少。

**Pass-Through Authentication** 这里有一些公司希望在本地 Active Directory 上对用户进行身份验证以强制遵守特定安全程序和密码策略的场景。此方法需要下载和配置身份验证代理，该代理管理 AAD 与本地 AD 之间的身份验证。

**Active Directory Federation Service (ADFS)** 在此场景中，所有用户身份验证都在本地进行。因此，ADFS 是一组彼此具有信任关系的域。希望与 AAD 建立信任关系的本地域管理员可以将本地环境与 Azure AD 联合。

要访问 Azure 门户中的单点登录功能，请单击 Azure Active Directory 链接并单击 Azure AAD Connect；你将看到当前配置状态，类似于图 2.15。

## FIGURE 2.15 AAD Connect state

![](img/95043b3df926bcdadadb082b49262037_94_0.png)

## B2B Collaboration

B2B 协作是一种企业对企业概念，允许一家公司与来自其他企业的访客用户共享服务和应用程序。连接是安全的，并且允许共享方对其自己的企业数据保持控制。如图 2.16 所示，B2B 协作的实施是通过单击 + New Guest User 按钮并邀请个人加入 Azure Active Directory 来完成的。

一旦访客用户被邀请加入，你就可以使用稍后讨论的条件访问功能来控制他们对企业内容的访问。此外，你可以将访客添加到 Azure Active Directory 和基于角色的访问安全角色中。因此，访客账户和 B2B 协作是让个人使用其当前身份进入你的 AAD 的简单方法。还存在一个 Azure AD B2C 功能，用于不同的用例。而 B2B 是为了让个人帮助协作项目或产品，B2C 功能则是为了对通过 B2B 概念创建的产品进行用户身份验证，例如。

## FIGURE 2.16 A list of users in Azure Active Directory

![](img/95043b3df926bcdadadb082b49262037_95_0.png)

## Self-Service Password

在出现像 Active Directory 这样的全球身份存储库之前，更改、重置和解锁账户为支持团队带来了大量工作。内部开发团队很快就收到了这一反馈，并在其应用程序中内置了更改、重置和解锁密码的自助功能。

在免费版中，唯一支持的功能是更改密码的能力。如果用户主要在 Azure 中，则在此处完成：myapps.microsoft.com。在某些标准使用场景中，自定义安全策略可以提供触发器，要求用户每 30 到 90 天更改一次密码。

重置和解锁账户的功能在免费版中可用；然而，它是自助式的——回到过去。在基础版和高级版中，内置了重置和解锁密码的自助功能。这是进行成本分析的最佳位置。换句话说，将你拥有的账户数量乘以每个用户的成本，并包括用户需要解锁或重置密码的概率。是该金额大于或等于在基础版或高级版中运行 Azure Active Directory 的成本吗？

## Application Proxy

这个功能让我想起了混合连接管理器（HCM），将在下一章中讨论，我们将详细讨论 Azure 网络。相似之处在于，有一个连接器服务，也称为连接管理器，可从 Azure 门户下载，通常安装在客户本地网络中的服务器上。导航到 Azure Active Directory 刀片并单击 Azure Proxy 链接会呈现用于下载连接器的链接，如图 2.17 所示。

## FIGURE 2.17 Application Proxy link

![](img/95043b3df926bcdadadb082b49262037_95_1.png)

