

|  | Azure Storage Queue | Service Bus |
|---|---|---|
| Push | 否 | 是 |
| 批量接收 | 是 | 是 |
| 批量发送 | 否 | 是 |
| 重复检测 | 否 | 是 |
| 最大队列大小 | 500TB | 1GB 到 80GB |
| 最大消息大小 | 64KB | 256KB 到 1MB |
| 仅 HTTPS | 否 | 是 |
| 自动死信 | 否 | 是 |
| 传递 | 至少一次 | 至少一次 |

在选择消息驱动的消息服务时，有很多选项需要考虑。你现在应该已经具备了朝着做出良好进展的方向迈进的条件。在下一节中，你将了解其他事件驱动的消息服务，然后能够在这两者之间做出选择，最终在全部四种之间做出选择。

## Event Grid

当我尝试描述 Event Grid 是什么时，我又回到了委托的定义。委托是一个编程术语，它是事件与方法之间的粘合剂。当事件被触发时被触发的方法通常被称为事件处理程序。事件与现实世界中的通知是同一回事。它是一条可能让许多监听者感兴趣的单条信息。

在深入细节之前，请先完成下一个练习，练习 6.6，在其中你将向练习 5.6 中之前创建的 Azure Blob Storage 容器添加一个事件。此事件将在向容器添加新 blob 时触发，然后触发你在练习 4.16 中创建的 Azure Function。

### EXERCISE 6.6

### 配置 Event Grid 服务以在 blob 添加到容器时触发

- 1. 登录到 portal.azure.com 上的 Azure 门户。在浏览器顶部中间的搜索框中，输入 Function App。现在，选择 Function App，然后选择你在练习 4.16 中创建的 Azure Function。单击 Functions 树旁边的 + 符号，如图 6.26 所示。在刀片上选择 Azure Event Grid Trigger 并单击 Install 按钮。为函数输入一个名称（例如，csharpguitar-eg）并单击 Create 按钮。

# FIGURE 6.26 Creating an Event Grid–triggered Azure Function

![](img/95043b3df926bcdadadb082b49262037_541_0.png)

# EXERCISE 6.6 (continued)

tree, as shown in Figure 6.26. Select Azure Event Grid Trigger on the blade and click the Install button. Enter a name for the function (for example, csharpguitar-eg) and click the Create button.

# FIGURE 6.26 Creating an Event Grid–triggered Azure Function

![](img/95043b3df926bcdadadb082b49262037_541_0.png)

2. 在你用于访问 Azure 门户的浏览器顶部中间的搜索框中，输入 Storage accounts 然后选择 Storage accounts。现在，选择你在练习 5.6 中创建的存储帐户。在导航菜单上选择 Events，单击 Storage accounts - Events 刀片上的 More Options 链接，然后选择 Web Hook。为 Event Hub 订阅添加一个名称，并仅从 Event Types 下拉列表中选择 Blob Created，如图 6.27 所示。从 Endpoint Type 下拉列表中选择 Azure Function 并单击 Select An Endpoint 链接。选择订阅、资源组、Function 应用和你在上一步中创建的函数。单击 Confirm Selection 按钮。

# FIGURE 6.27 Configuring an Azure Blob Storage container to trigger an Event Hub event when created

![](img/95043b3df926bcdadadb082b49262037_541_1.png)

3. 导航回你创建的 Azure Function 并展开 Log 窗口，如图 6.28 所示。Log 窗口在你添加下一个步骤中的 blob 之前将是空的。一旦 blob 被添加，详细信息将被发送到 Event Grid 然后发送到 Azure Function。

# FIGURE 6.28 Azure Function log showing Event Grid and blob details

![](img/95043b3df926bcdadadb082b49262037_542_0.png)

4. 使用如图 5.53 所示的相同命令向 blob 容器添加一个 blob。检查 blob 容器的内容以确认其存在。你可以通过从 Azure 门户导航到它来实现。查看 Azure Storage Account Event 刀片上的 Event Subscriptions 指标，如图 6.29 所示。

# FIGURE 6.29 A view of an Azure Storage Event Grid Event metrics

![](img/95043b3df926bcdadadb082b49262037_542_1.png)

在开始学习一些 Event Grid 细节时，请查看表 6.1，然后继续学习这些最常见的 Event Grid 术语：

- - Event, publisher, source, and handler
- - Domain, subscription, and topic

在之前的练习中，认识到事件（又名通知）来自于向受监控的源 Azure Storage Blob 容器创建 blob，因为事件就发生在这里。再次注意图 6.27 中你选择了 Blob Created，这指示源将事件通知发送到 Event Grid。Event Grid 然后将事件连同详细信息一起发送，如图 6.28 所示发送到处理程序——一个 Azure Function。在练习 6.5 场景中，你有以下内容：

- - **Event:** Blob Created
- - **Source:** Azure Storage
- - **Handler:** Azure Function

如图所示 6.30，许多 Azure 产品都内置支持通过给定 Azure 产品的导航菜单上的 Events 链接配置和订阅 Event Grid。

# FIGURE 6.30 A view of event grid sources and handlers

![](img/95043b3df926bcdadadb082b49262037_543_0.png)

如果要发送事件的 Azure 产品是 Event Grid 且没有内置订阅 Event Grid 的能力，那么你需要手动创建 Event Grid Topic。自定义事件的创建也是如此。当你创建 Event Grid Topic 时，它会公开一个全局端点，类似于以下内容：

```
`<topicName>.westeurope-1.eventgrid.azure.net/api/events`
```

Event Grid Topic 公开了一个 API，你可以向其发送包含有关事件的信息以及 Event Grid 转发给

controls to manage your workflows. Connectors can be Office 365 Outlook, SharePoint Online, Azure Blob Storage, and Dynamics 365 to name a few. Think for a moment about how you could use those connectors to implement the workflow process identified in Figure 6.31. You would use Outlook to send requests for approval and approval emails. You could use SharePoint online as a place to store details about the changes. Azure Blob Storage can store the actual Azure Policy JSON document. Perhaps even, after approval, you could insert an Azure Blob to trigger an Azure Function and then perform the deployment to production. Figure 6.32 shows an example of the Logic Apps Designer blade in the Azure Portal. It illustrates a two-step process to send an email when a new file is added, modified, or some content is changed on a SharePoint site.

# FIGURE 6.32 Business process workflow diagram example

![](img/95043b3df926bcdadadb082b49262037_545_0.png)

A trigger is some action or event that kicks off the workflow such as an HTTP request to an API from a client, scheduled or from another Logic App. In the workflow scenario example, this may be triggered via an email sent to a specific email alias that contains the requested change or creation to the Azure Policy. That email then triggers notifications to those who can work on, test, and ultimately approve the application of the policy into your Azure subscription. This interface reminds me of a programming technique called Snap! (snap.berkeley.edu) where you can drag and drop logical controls into a graphical interface, separated by actions such as for each, if-then-else, switch cases, and while loops. The same is possible with Logic Apps and would be necessary to implement in the workflow example used here. In short, you use the graphical user interface to drag and drop the connections and the actions required to successfully process your workflow, and the code is generated for you behind the scenes.

Also, you would use Azure Logic Apps when you want to automate workflows that have historically been performed manually, like gaining expense approvals, hiring new employees, vacation approvals, or the fulfillment of a purchase order. The days of having a large stack of papers waiting in an inbox or emails waiting in an inbox for approval are over. These types of activities can be automated using Azure Logic Apps. It is one of the driving forces behind the concept of digitalization of the enterprise and workforce.

## Notification Hubs

This tool is used to send push notifications to mobile devices running your mobile app. Have you ever received a notification on your phone that someone sent you a What’s App text, liked one of your posts on Facebook, or that the stock market climbed over 1,500 points? Well, either they used Notification Hub to perform that or they used a product similar to it. Notification Hub supports the sending of pop-up messages to clients running Android, iOS, and Windows.

Notification Hubs are tightly integrated with App Service Mobile Apps, which was introduced in Chapter 4. In that section, you learned that Mobile Apps is deprecating. However, it is important for legacy and potentially an exam question perspective to know at least a little about the integrated Azure products, Mobile Apps, and Notification Hub.

## Summary

This chapter provided the finishing touches to understand a hybrid solution. The concept was touched in many chapters before this one, and in this chapter you got clarity about security, networking, compute, and data hybrid solutions. Hybrid solutions are the combination of resources existing in cloud and private data centers. Azure Blueprint, Azure Policy, and Security Center are all tools that can help you meet compliance standards such as GDPR, PIPA, and ISO/IEC 27018. These tools help by providing an outline of what Azure procedures are required to meet a certification standard specifically one like Azure Blueprint. Azure Policy provides the means for applying and enforcing those described procedures and Security Center monitors and alerts and provides feedback on noncompliant configurations in near real time.

In the second part of this chapter, you learned about Azure’s MaaS offerings and some message patterns used to decouple large IT solutions. Decoupling makes a computer system more reliable, as the dependency between previously linked systems are detached from each other. Instead of placing an order directly into an OLTP database, the order can be placed into a queue and processed offline. You learned why you would use Service Bus, Azure Storage queues, Event Hub, or Event Grid. Messaging is not a new concept and is something that is getting more traction as legacy applications retire and are refreshed with new technology and design patterns. The digital transformation happening in many companies and one of Microsoft’s major areas of operations can be mostly achieved by digitizing historically manual workflows with Azure Logic Apps.

