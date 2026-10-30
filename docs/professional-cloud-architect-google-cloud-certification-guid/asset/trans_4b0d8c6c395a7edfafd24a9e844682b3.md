

# 模拟测试二

1. 公司 X 想要一个标准化、可重新部署的 Hadoop 集群，并且需要托管服务所不提供的选项。哪种解决方案最适合？
A. Cloud API
B. Deployment Manager
C. Dataflow
D. TensorFlow

2. 公司 X 希望将其后端平台连接到托管的 NoSQL 数据库服务。预计数据库可能会增长到 PB 级别。作为架构师，他们询问您哪种 GCP 服务最能满足这些要求而无需重构任何应用程序。最佳选择是什么？
A. MySQL
B. Bigtable
C. Firebase
D. Redis

3. 选择服务账号的不同类型（选择三项）：
A. User-managed
B. Automated
C. Google-managed
D. G Suite
E. Google APIs

4. 公司 X 有两个项目，由不同的 VPC 分隔，需要能够相互通信。哪种网络服务允许这样做？
A. VPC peering
B. Cloud Load Balancing
C. Dedicated Interconnect
D. VPN

5. 公司 X 希望在云中使用容器。他们希望继续以开发者为中心并采用代码优先的策略。最佳解决方案是什么？
A. App Engine standard
B. Containers on Compute Engine
C. Cloud Run
D. App Engine flexible

6. 您的 IT 经理正在查看云厂商的数据存储服务。他的 DBA 告知他主要要求是强一致性和高可用性，并且有潜力增长到 PB 级别。最佳存储解决方案是什么？
A. Cloud SQL
B. Cloud Storage
C. Cloud Datastore
D. Cloud Spanner

7. 公司 X 需要符合 PCI 标准。哪种 GCP 服务组合有助于满足这些要求？
A. Cloud Monitoring、Cloud Trace 和 Cloud Spanner
B. Cloud Monitoring、Cloud Logging 和 BigQuery
C. Cloud Error Reporting、Cloud Debugger 和 Datastore
D. Cloud Tagging、Cloud Trace 和 BigQuery

8. 公司 X 的存储工程师需要将数据从其 AWS S3 存储桶迁移到其 GCP 存储桶。最佳解决方案是什么？
A. Storage Transfer Service
B. Transfer Appliance
C. Online transfer
D. BigQuery data transfer

9. 一家公司网页面向全球用户提供服务。他们希望确保用户无论身在何处都能以最有效的方式获取内容。哪种负载均衡解决方案最能满足这些要求？
A. Network Load Balancing
B. Internal Load Balancing
C. HTTP(S) Load Balancing
D. TCP Proxy Load Balancing

10. 公司 X 希望通过云实现自动扩缩容。他们希望以标准化方式跨多个区域部署，同时受益于负载均衡。哪种 GCP 服务最适合此场景？
A. Deployment Manager
B. Managed instance groups
C. Google Compute Engine manager
D. Instance fleet

11. 您正在创建新的防火墙规则，并希望根据其用途识别特定目标，例如 Web 服务器。您应该使用哪种过滤器？
A. Zones
B. Network tags
C. Instance groups
D. Targets

12. 您已将实例部署到与现有实例相同的 VPC 中。当您尝试使用 SSH 连接到外部 IP 地址时，连接被拒绝。为什么会这样？
A. 允许 SSH 的防火墙规则仅限于内部流量。
B. 实例未分配外部 IP。
C. 您没有启动 SSH 所需的正确自定义身份和访问管理（IAM）角色。
D. 您应该使用 Google API 进行外部 SSH。

13. 目前，您的 IT 部门在公司内部工时应用程序发布新软件更新时报告了大量错误。这些错误在 QA 期间未被发现。您被要求设计一种新策略，以将错误降至最低并恢复对 IT 部门的信心。哪种选项最适合此场景？
A. 建议您每年仅部署一次更新。
B. 仅将更新的一部分部署到生产环境。
C. 在 QA 期间多次执行测试。
D. 使用金丝雀部署方法。

14. 您的公司希望将其本地网络连接到 GCP VPC，以便在各站点之间动态交换路由。您会建议哪种服务？
A. Cloud Router
B. Cloud Interconnect
C. External peering
D. Cloud DNS

15. 您计划使用 VPC peering 连接 VPC 网络。哪种网络模式最适合？
A. Auto mode networks
B. VPC VPN networks
C. Custom mode
D. Sub-networking mode

16. 您被要求研究将本地网络扩展到 GCP VPC 网络的不同方法。您的经理提醒您，您的网络带宽为 1 Gbps。最佳选择是什么？
A. Dedicated Interconnect
B. Partner Interconnect
C. VPC Interconnect
D. VPN Interconnect

17. 公司 X 希望将数据中心扩展到云端。您受聘为外部顾问，负责建议最佳混合连接选项。他们告知您，他们需要访问 GCP 上的私有计算资源，但不担心应用层加密。哪种选项最符合他们的需求？
A. Cloud VPN
B. Partner Interconnect
C. Direct Peering
D. Carrier Peering

18. 您希望以低延迟在全球范围内提供所有内容。您应该使用哪种 GCP 服务？
A. Cloud CDN
B. Cloud VPN
C. Google CloudFront
D. Cloud Endpoints

19. 您希望根据传入端口对系统进行负载均衡。您应该使用哪种负载均衡概念？
A. Network Load Balancing
B. TCP Load Balancing
C. HTTP(S) Load Balancing
D. SSL Proxy Load balancing

20. 您希望允许访问发布消息到 Cloud Pub/Sub 主题。您的安全团队提醒您应尽可能精细。您应该使用哪种类型的 IAM 角色？
A. Primitive role
B. Predefined role
C. Custom role
D. Policy-based role
E. Topic role

# 模拟测试三

1. 您使用 Cloud Build 服务构建容器镜像。您希望访问诸如其镜像构建所在的 Google Cloud 项目等信息。推荐的方式是什么？
A. 在构建配置文件中使用替换项，在构建时替换特定变量。
B. 在构建中运行 gcloud 命令。
C. 运行 API 调用以获取信息。
D. 无法访问此信息。

2. 您在 Linux 发行版上运行 Web 应用程序。您希望完全消除修补操作系统的开销。哪种选项最符合您的要求？
A. 将应用程序容器化并使用托管的基础镜像。
B. 使 VM 成为只读。
C. 为您的 VM 使用 Alpine（精简）镜像。
D. 使用 Google 的操作系统修补服务。

3. 您被要求将经典的 LAMP 应用程序容器化。您应遵循的最佳实践是什么（选择两个选项）？
A. 每个容器打包一个应用程序。
B. 将所有应用程序打包到单个容器中。
C. 删除不必要的工具。
D. 使用公共镜像。

4. 您已将应用程序迁移到 GKE，并希望了解应用程序对由单个微服务不可用引起的故障的反应。最简单的方法是什么？
A. 编写脚本随机杀死微服务。
B. 在 GKE 集群上启用 Istio 并使用故障注入。
C. 部署应用程序，使用编辑过的 YAML 文件删除与要测试的微服务相关的代码。
D. 将微服务移至 GCE VM 并在测试期间关闭 VM。

5. 您正在为应用程序添加新功能，并决定使用 Cloud Functions。您的应用程序在 VPC 内的 GCE VM 上运行。Cloud Function 需要直接网络访问该 VM。如何实现？
A. 使用 private services access。
B. User Serverless VPC Access。
C. 使用 Private Google Access。
D. 使用 Private Service Connect for Google APIs。

6. 您希望根据自己创建的自定义指标来扩缩容托管实例组（MIG）。哪种选项符合您的要求？
A. 创建 Cloud schedule 和 Cloud function 查询指标然后扩缩容 MIG。
B. 创建 Cloud operations suite 警报以触发扩容事件。
C. 设置 MIG 从组中所有 VM 导出自定义指标。
D. MIG 不支持自定义指标。

7. 您希望衡量托管在 GKE 集群上的 Web 应用程序的性能。哪种选项最符合您的需求？
A. 定期对用户进行客户满意度调查。
B. 创建 Cloud Monitoring 健康检查。
C. 使用第三方 SaaS 解决方案定期检查 HTTP 400 响应。
D. 使用 Cloud Monitoring 定义并设置 SLI 和 SLO。

8. 您希望确保只有经过身份验证和授权的用户才能访问运行在 GKE 集群上的应用程序。Google 推荐的设计是什么？
A. 创建具有专有身份验证和授权机制的前端容器。
B. 使用 Identity-Aware Proxy 和 GKE Ingress。
C. 将应用程序移至 Cloud Run 并设置授权。
D. 使用 LDAP 将应用程序与 Active Directory 集成。

