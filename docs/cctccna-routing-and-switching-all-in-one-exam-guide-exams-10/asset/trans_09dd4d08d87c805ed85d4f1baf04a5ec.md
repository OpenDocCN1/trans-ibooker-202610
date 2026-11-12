
负责分析问题，显示有关问题的信息，然后为问题提供解决方案。

## 北向 API 和南向 API

*应用程序编程接口*（API）是包含若干执行不同操作的函数的应用程序逻辑。在 SDN 网络中，API 对于将信息从用于管理更改的应用程序传输到控制器，然后从控制器传输数据到网络设备至关重要。API 是移动数据的*逻辑*，但数据本身处于一种*数据格式*中，该格式使其能够跨应用程序和设备共享。（您稍后将了解更多有关数据格式的内容。）

对于 Cisco CCNA 考试，您需要熟悉 SDN 网络中的两个 API：*北向 API* 和 *南向 API*。如图 18-1 所示，北向 API 负责在应用程序和控制器之间移动数据，而南向 API 负责在控制器和网络设备之间移动数据。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1050_0.png)

图 18-1 北向 API 和南向 API

API 使用客户端/服务器架构，每次执行都需要客户端和服务器。客户端是接收数据的组件，服务器是发送数据的组件。因此，对于北向 API，服务器是应用程序，客户端是控制器。对于南向 API，服务器是控制器，客户端是网络设备。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1050_1.png)

**考试提示** 对于 Cisco CCNA 考试，请熟悉北向 API 和南向 API。

API 有两种不同类型：*内部 API* 和 *Web 服务 API*。内部 API 用于在系统上的应用程序之间共享数据，而 Web 服务 API 用于在网络或 Internet 上的设备之间交换数据。

## 数据交换

在 SDN 网络中，需要在用于将配置设置应用于设备的应用程序、SDN 控制器和网络设备之间交换配置设置。在本节中，您将了解该数据交换的发生方式以及数据格式。

## 基于 REST 的 API

最常见的 API 类型之一是*表述性状态转移*（REST）API。这种 Web 服务 API 主要用于北向 API。在基于 REST 的 API 中，HTTP 方法对数据执行创建、读取、更新和删除（CRUD）操作。例如，HTTP **GET** 动词（即方法）用于读取数据，而 HTTP **POST** 动词用于创建数据。以下是 REST API 使用的一些 HTTP 动词：

- **GET** 用于执行从源*读取*数据的调用。例如，以下 URL 可能会检索客户 5 的详细信息：
- **PUT** 用于*更新*数据中的现有记录。例如，以下 URL 将更新客户 2：
- **POST** 用于*创建*数据中的新记录。例如，以下 URL 将创建客户 10：
- **DELETE** 用于从数据中*删除*记录。例如，以下 URL 将删除客户 10：

```
GET http://www.gleneclarke.com/customers/5
```

```
PUT http://www.gleneclarke.com/customers/2/Glen/Clarke/Halifax
```

```
POST http://www.gleneclarke.com/customers/10/Bob/Smith
```

## 数据格式

如前所述，API 是对数据执行的操作，但数据需要处于一种便于在平台、应用程序和设备之间共享的格式中。这就是两种常见数据格式（也称为数据编码方法）发挥作用的地方：XML 和 JSON。

### XML 数据格式

可扩展标记语言 (XML) 是一种常见格式，用于需要交换的数据，以便数据在目标端可被理解。XML 在 2000 年代初非常流行，并且今天仍被用于交换数据。

XML 易于阅读，因为它使用类似于 HTML 标签的标签来描述数据。查看以下 XML 代码，您可以看到数据中有多本书籍，第一本书的标题是 CompTIA Security+ Certification Study Guide，第二本书是 CompTIA Network+ Certification Study Guide：

```
<Books>
    <Book ISBN="1260026051">
        <Title>CompTIA Security+ Certification Study Guide</Title>
        <Author>Glen E. Clarke</Author>
        <Price>34.76</Price>
    </Book>
    <Book ISBN="1260122042">
        <Title>CompTIA Network+ Certification Study Guide </Title>
        <Author>Glen E. Clarke</Author>
        <Price>37.26</Price>
    </Book>
</Books>
```

请注意，两本书的作者很清楚是 Glen E. Clarke。使用标签可确保数据接收方不会误解信息（换句话说，接收方不会将作者 Glen E. Clarke 与书籍标题混淆）。

### JSON 数据格式

多年来，XML 一直是在环境之间交换信息的数据格式，但不断打开和关闭标签增加了正在传输的数据量。随着移动应用程序的普及，需要一种新的数据格式来尽量减少传输的数据量，尤其是通过蜂窝电话网络：于是 JavaScript 对象表示法 (JSON) 登场了。

与 XML 相比，JSON 被认为是一种轻量级数据交换协议。现在，许多应用程序和 API 都在使用它来在环境之间共享数据。以下是我们书籍数据的 JSON 格式示例：

```
{ "books": [
  { "isbn":"1260026051",
    "title":"CompTIA Security+ Certification Study Guide",
    "author":"Glen E. Clarke",
    "price":"34.76"},
  { "isbn":"1260122042",
    "title":"CompTIA Network+ Certification Study Guide",
    "author":"Glen E. Clarke",
    "price":"37.26"}
]}
```

关于 JSON 格式，请注意几点——因为您确实需要知道这些以应对考试：数据表示为名称-值对，标签位于引号内，然后是冒号，然后是该数据的值，也位于引号内。每个数据项之间用逗号分隔，每条记录都位于花括号 { } 中。例如，请注意每本书的信息都位于一组花括号中。还请注意，整个列表位于两组括号内：花括号和方括号，{ [ ] }。这是因为 JSON 数据需要花括号 { }，但由于此列表中有多本书籍，因此使用数组，并由方括号 [ ] 标识。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1053_0.png)

**考试提示** 对于 CCNA 考试，请了解 JSON 数据格式。

## 配置管理技术

在讨论 SDN 环境时，我提到应用程序与 SDN 控制器通信，然后由控制器将配置更改发送到网络设备。这些应用程序称为*配置管理系统*，用于自动对网络设备进行配置更改。配置管理系统使用您开发的脚本，以同时向多个设备应用配置更改。脚本创建后，您可以使用配置管理系统的 GUI 来安排在设备上运行脚本的任务，也可以手动将配置更改应用到设备。在配置管理系统领域，您应该记住的三大主要参与者是 Puppet、Chef 和 Ansible。

请注意，这些程序都使用我们之前讨论过的 API 来传达所需的更改。例如，配置管理应用程序（Puppet、Chef 或 Ansible）使用北向 API 将配置更改发送到控制器，然后控制器使用南向 API 将更改发送到设备。

### Puppet

Puppet 使用主/代理拓扑，其中主节点是控制器，您在要管理的每个网络设备上安装一个代理。Puppet 使用*拉取模型*，这意味着代理会定期向主节点（控制器）检查是否有任务需要运行。如果找到此类任务，代理会从主节点拉取脚本（称为*清单文件*）并在设备上运行它。Puppet 使用的脚本语言是 Ruby，主节点与设备通信的通道是 TCP 端口 8140。

### Chef

Chef 与 Puppet 类似，它也是主/代理拓扑，并且也会检查需要运行的任务。同样，如果找到此类任务，代理会从主节点（控制器）拉取更改脚本（在此示例中称为*食谱*）并应用配置
