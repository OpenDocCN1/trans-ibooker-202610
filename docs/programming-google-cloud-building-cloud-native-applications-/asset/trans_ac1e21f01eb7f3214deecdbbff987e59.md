

# 查找资源类型和指标？

资源类型：Dataflow Job
指标：元素计数

图 5-14. Cloud Logging 指标选择

4. 你应该在右侧窗格看到如图 5-15 所示的图表。

![](img/eac8762c24ec77948aa0c3277f8e6eb3_159_0.png)

图 5-15. Cloud Logging 元素计数指标图

Cloud Monitoring 还允许你对多个资源进行分组。

+   1. 在 Groups 菜单中，选择 Create Group
+   2. 输入你的组名称，例如 Speech Analytics Dataflow Group
+   3. 添加筛选条件，如图 5-16 所示。

## Criteria

## Add criterion

Type *
Name

Operator *
Starts with

Value *
beam

CANCEL   DONE

图 5-16. Cloud Logging 组条件选择

4. 点击 Create

5. 你现在应该可以看到你的组已创建，所有 Apache Beam 管道都已分组到单个仪表板中，如图 5-17 所示

![](img/eac8762c24ec77948aa0c3277f8e6eb3_162_0.png)

图 5-17. Cloud Logging 分组

6. 在 Alerting 菜单中，点击 Create Policy。

7. 在 Create new alerting policy 页面中，定义告警条件和通知渠道。例如在图 5-18 中，条件已设置为 System Lab for Dataflow：

+   8. 图 5-18 Cloud Logging 告警策略
+   9. 输入所需的值，包括通知渠道。
+   10. 完成后你可以查看你的告警策略示例

图 5-18

![](img/eac8762c24ec77948aa0c3277f8e6eb3_163_0.png)

图 5-18. Cloud Logging Dataflow 系统延迟告警策略

Cloud Monitoring 为你提供了一套全面的工具来保持管道处于健康状态，并在指标达到你设定的阈值并需要关注时提醒你。

## Cloud Native Checkpoint

让我们回顾一下我们为云原生架构列出的考虑因素，以及我们如何通过 Cloud Dataflow 来解决每个问题。

+   1. 你的应用程序需要从云基础设施中抽象出来。这里你使用的是 Google Cloud Dataflow Runner。选择 Cloud Dataflow 作为运行器可以让你专注于管道，并允许 Cloud Dataflow 按需管理资源并自动扩缩容以实现最低延迟和高资源利用率。其思想是相同的管道可以在不同的运行器上执行，从而提供使用不同运行器的可能性和选择。✓
2. 你需要一种持续集成和持续交付的方法。你尚未学习如何将 CI/CD 集成到管道部署中。这超出了本章的范围，但推荐的方法是利用多个部署管道。例如：测试、暂存和生产。你可以在部署前对代码执行单元测试，以在部署到生产环境前验证管道的健康状态。在第 8 章中，你将学习如何将 CI/CD 集成到管道部署中。✓
3. 应用程序必须能够根据需要进行扩容和缩容。Cloud Dataflow 按需管理资源并自动扩缩容以实现最低延迟和高资源利用率。✓
4. 应用程序将如何管理故障？管理故障的关键在于你主动监控应用程序和 Cloud Dataflow。在本章中，你学习了如何查看指标并为 Cloud Dataflow 启用日志记录以提供健康的管道。✓
5. 应用程序需要能够全球访问。对于我们的数据处理管道来说，这并不相关。如果你在管道中检测到故障，你可以轻松地将其部署到另一个区域或地区。由于 Cloud Pub/Sub 主题可以让消息存储在多个区域，因此你在队列中提供了弹性。Cloud Dataflow 可以在管道故障时从中断处继续。✓
6. 我们的框架需要基于微服务架构。尚不适用，但在你将应用程序部署到 Kubernetes 后就会适用。
7. 我们的应用程序将如何得到保护？Cloud Dataflow 使用 Google Cloud 服务账号进行保护。✓

## Closing Remarks

在本章中，你使用 Cloud Dataflow 部署了数据管道，并学习了如何通过使用 Cloud Monitoring 来保持 Cloud Dataflow 健康。在下一章中，你将学习如何使用 Google Cloud Machine Learning API 通过 Google Cloud 服务丰富数据。

## About the Authors

**Rui Santos Costa** Rui Costa 在 Google 担任过多个角色，最近担任学习顾问，与战略客户和合作伙伴合作，创建并执行他们的 Google Cloud 学习计划。他曾担任 Google AI Impact Challenge 的 AI 教练，也是 Speech Analysis Framework 的创始人，该框架已成功毕业成为 Google 内部的产品。

**Jasen Baker** 拥有超过 20 年的 IT 经验，从 1993 年从事“在互联网上开展业务的商业公司”的工作开始，到 1997 年创办一家网络托管公司。Jasen 在多个初创公司担任 Unix 工程师 15 年后，在 Nike、HP 最终进入 Google 担任技术职位。他的职业生涯从系统管理开始，发展到售前，再到培训，Jasen 喜欢从事引人入胜的职业，教授新一代技术人员有关云服务和软件开发的转型。除了技术培训外，Jasen 还喜欢复古游戏、股票交易和旅行。

