

## 19

## 测试你的知识

是时候测试你所学知识了。以下是四个模拟考试来帮助你。祝你好运！

## 模拟测试一

1. Company X 需要将数据保留 5 年以满足审计要求。他们计划每年访问此存储不超过一次。应选择哪种存储选项？
- A. Google Cloud Bigtable
- B. Google Cloud Multi-Regional Storage
- C. Google Cloud Archive Storage
- D. Google Cloud Nearline Storage
- E. Google Cloud BigQuery

2. Company X 希望为物联网传感器数据选择合适的存储系统。有 2,000 个传感器每秒发送温度数据。Company X 希望对累积的数据进行进一步分析。请选择最合适的选择：
- A. Google Cloud Bigtable
- B. Google Cloud Datastore
- C. Google Cloud Spanner
- D. Google Cloud SQL

3. 你已在项目 X 中部署了一个虚拟机实例到 GCP。此实例上已安装特定配置和软件。为了将此镜像与其他只能访问项目 Z 的团队共享，你会建议什么？
- A. 创建快照。使用快照创建自定义镜像。将镜像与其它项目共享。
- B. 创建快照并将其存储在 Google 存储上。
- C. 使用第三方工具对实例执行文件级备份。将镜像复制到 Google 存储。将镜像导入项目 Z。
- D. 使用 Google Transfer Services。

4. Company X 希望分析数据。他们使用的是本地和 GCP 基础设施混合的混合云，并且需要分析流数据和批处理数据。请选择能够满足此要求的适当 GCP 服务：
- A. Google Cloud Dataproc
- B. Google Cloud BigQuery
- C. Google Cloud Compute Engine and Apache Airflow
- D. Google Cloud Dataflow

5. Company X 使用 Hadoop 分析数据。他们使用的是本地和 GCP 基础设施混合的混合云。他们希望将数据分析迁移到 GCP，但希望以最小的努力进行迁移。应使用哪项服务？
- A. Google Cloud Dataproc
- B. Google Cloud Dataflow
- C. Google Cloud Composer
- D. Google Compute Engine

6. Customer X 将数据存储在 Google Datastore 上。他们使用的是本地和 GCP 基础设施混合的混合云。两个平台上的应用程序都需要访问 Datastore。应使用哪种解决方案来启用访问？
- A. 对 GCP 实例使用 Google 管理的密钥。对本地实例使用用户管理的密钥。
- B. 对所有实例使用 Google 管理的密钥。
- C. 对 GCP 实例使用 Google 管理的密钥。对本地实例使用 Firebase 身份验证。
- D. 对 GCP 实例使用 Google 管理的密钥。对本地实例使用第三方工具。

7. Company X 在 GCP 中使用多个配置的项目。他们对计费可见性和管理有特殊要求。根据以下陈述，选择适当的答案：CTO 应该能够控制不同项目的预算，而项目经理只能查看其项目的计费信息。
- A. 将计费管理员角色设置为 CTO 管理的所有项目。将计费查看者角色设置为其项目的项目经理。
- B. 将计费管理员角色设置为随机项目的项目经理。将计费查看者角色设置为其项目的项目经理。
- C. 将计费管理员角色设置为项目和项目经理。
- D. 将所有者角色设置为项目和项目经理。

8. 你正在使用正常运行时间检查监控一项服务。服务被报告为从不同的 GCP 区域不可用。你知道服务正在运行。如何解决监控问题？
- A. 从正常运行时间检查控制台下载源 IP 并为服务创建入站防火墙规则。
- B. 从正常运行时间检查控制台下载源 IP 并为服务创建出站防火墙规则。
- C. 使用 GCP 外部的第三方工具创建正常运行时间检查。
- D. 在托管该服务的所有实例上安装 Cloud operations 监控代理。

9. Company X 希望在 GCP 中创建开发和生产环境。分离这些环境的最佳实践是什么？
- A. 为每个环境创建两个单独的项目。仅向开发团队授予对开发项目的访问权限。仅向运营团队授予对生产环境的访问权限。
- B. 为每个环境创建两个单独的项目。向开发团队和生产团队授予对两个项目的访问权限。
- C. 创建一个项目和两个 VPC。向开发团队和生产团队授予对该项目的访问权限。
- D. 为每个团队创建两个单独的 Google 账号。

10. Company X 希望分析来自传感器的传入数据。数据可能乱序到达。你需要确保数据按正确顺序排列。应使用哪些服务以最小化工作量？
- A. IoT Core、Pub/Sub 和 Dataflow
- B. IoT Core、Pub/Sub 和 Dataproc
- C. IoT Core、Pub/Sub 和 Google Kubernetes Engine (GKE)
- D. IoT Core、Pub/Sub 和 GCE

11. Company X 使用 App Engine 部署了应用程序。他们希望将该应用程序的新版本发布到生产环境。他们希望仅对一组用户进行测试。最合适的解决方案是什么？
- A. 部署应用程序的新版本。使用流量拆分将部分请求重定向到新版本。
- B. 将应用程序部署到单独的项目并指示用户使用新的 URL 连接到它。
- C. 将应用程序迁移到 GKE 并使用蓝绿部署。
- D. 将应用程序迁移到 GKE 并使用滚动更新。

12. Company X 使用 App Engine 灵活环境。他们部署了应用程序的新版本。应用程序崩溃了。代码存储在 GitHub 上。如何进行最快的恢复？
- A. 删除新应用程序并从 GitHub 部署新应用程序。
- B. 将应用程序回滚到之前的版本。
- C. 在旧版本和新版本之间拆分流量，比例为 10% 到 90%。
- D. 向 GCP 支持开票以将应用程序回滚到上一个版本。

13. Company X 使用 Google Cloud 的 operations suite 来监控其 GCP 环境。他们希望存储日志并能够分析它们。对他们来说最好的解决方案是什么？
- A. 创建到 Pub/Sub 的接收器。
- B. 创建到 Spanner 的接收器。
- C. 创建到 BigQuery 的接收器。
- D. 创建到 Bigtable 的接收器。

14. Company X 使用 GKE 集群。你希望增加集群中的节点数量。运行哪条命令最合适？
- A. 运行 `gcloud container clusters increase` 命令以更改节点数量。
- B. 运行 `gcloud container clusters resize` 命令以更改节点数量。
- C. 运行 `kubectl container cluster scale` 命令以更改节点数量。
- D. 运行 `gcloud container cluster resize` 命令以更改节点数量。

15. Company X 希望将 MySQL 数据库迁移到云端。他们希望使用托管服务。请选择最合适的选择。
- A. 使用 Compute Engine 实例并部署 MySQL。
- B. 使用 App Engine 实例并部署 MySQL。
- C. 使用 Cloud SQL。
- D. 使用 Cloud Spanner。

16. Company X 正在创建一个应用程序来分析其 Facebook 资料上的评论。他们希望以最简单的方式分析是否存在负面评论。应使用哪项服务？
- A. TensorFlow
- B. Google AutoML
- C. Google ML Engine
- D. Natural Language API

17. Company X 希望利用 ML 根据过去的数据估算材料成本。他们应使用哪种类型的模型？
- A. 回归
- B. 分类
- C. 多类分类模型

18. Company X 希望为项目预算设置警报。实现此目的的最佳方式是什么？
- A. 使用所需百分比创建预算警报。
- B. 向 Google 支持开票以设置硬配额。
- C. 创建 cron 作业以检查计费，并在超过阈值时发送电子邮件。
- D. 对附加到账户的信用卡设置限制。

19. Company X 希望将数据存储在 Cloud Storage 中。数据每季度访问一次。一年后，数据将被归档。最具成本效益的解决方案是什么？
- A. 将数据存储在多区域存储桶中。设置 365 天的自动归档策略。
- B. 将数据存储在区域存储桶中。设置 365 天的自动归档策略。
- C. 将数据存储在 Nearline 存储桶中。设置对象生命周期策略，将数据在 365 天后移动到 Archive 存储桶。
- D. 将数据存储在 Nearline 存储桶中。创建 cron 作业，在 365 天后将数据移动到 Archive 存储桶。

20. Company X 希望设置一个静态网站。最快且最具成本效益的解决方案是什么？
- A. 使用 Cloud Launcher 部署 Apache Server。
- B. 使用预定义 Web 服务器的 App Engine。
- C. 使用 Cloud Compute Engine 和启动脚本安装 Apache Server。
- D. 使用 Cloud Storage 托管内容。

