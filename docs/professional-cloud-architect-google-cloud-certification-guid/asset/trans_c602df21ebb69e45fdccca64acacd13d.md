
9. 你想通过公共互联网使用 RDP 和 SSH 安全地连接到你的 GCE 虚拟机。最佳实践是什么？
A. 使用 IAP TCP 转发。
B. 为所有虚拟机设置公共 IP 地址。
C. 使用 VPN 隧道连接到 VM。
D. 在虚拟机上安装第三方远程桌面工具。

10. 你的公司位于 GCP 区域可用的欧洲国家之一。你将为该国家的客户提供服务。你希望确保不允许在该 GCP 区域之外部署资源。这如何实现？
A. 使用组织策略。
B. 在 Google 控制台的 Admin 菜单中禁用其他区域。
C. 使用 gcloud regions disallow 命令禁用其他区域。
D. 在结算帐号设置中禁用其他区域。

11. 根据 CIS 基准，你希望禁止某些虚拟机使用外部 IP 地址。这如何实现？
A. 使用组织策略。
B. 在 GCP 项目上设置元数据。
C. 将 VPC 配置为私有。
D. 将子网配置为私有。

12. 你希望你的云原生应用程序能够以安全的方式访问 GCP 服务。Google 推荐的做法是什么？
A. 将 Google 服务帐号令牌存储在 Kubernetes Secrets 中。
B. 将 Google 服务帐号令牌存储在容器镜像中。
C. 将 Google 服务帐号令牌存储在私有容器注册表中。
D. 使用 Workload Identity。

13. 你计划为你的新客户部署一个登陆区。客户希望确保网络团队和计算团队之间有明确的职责分离。你将使用哪种架构？
A. 单主机项目中的共享 VPC 和多个服务项目
B. 每个项目一个 VPC
C. 单个 VPC 和多项目，通过 VPC 对等连接到该单个项目
D. 单个 VPC 和多项目，通过 VPN 隧道连接到该单个项目

14. 你想在容器中运行你的应用程序并能够在混合云和多云环境中迁移它。默认情况下，应用程序将在 GCP 中运行。你希望确保应用程序能够自动扩展。你应该选择哪项服务？
A. Google Compute Engine
B. Google App Engine
C. Google Cloud Run
D. Google Cloud Functions

15. 你在混合环境中运行云原生工作负载，使用 GKE 和本地 Kubernetes 集群。你希望确保 Kubernetes 集群以统一的方式配置。这如何实现？
A. 在每个集群上应用 Kubernetes ConfigMaps。
B. 将本地集群附加到 Anthos 并使用 Config Management。
C. 在所有集群上安装 Istio 并使用 CRDs。
D. 在所有集群上安装 Config Connector 并使用 CRDs。

16. 你正在设计一个云原生应用程序，该应用程序将存储需要由在 Google Cloud 中运行的其他应用程序查询的数据。你决定使用 Cloud Run。最佳存储选项是什么？
A. 将数据存储在 GCS 存储桶上。
B. 将应用迁移到 GKE 而不是使用 PVC。
C. 从 Cloud Run 连接到 Cloud SQL 服务。
D. Cloud Run 不允许你存储持久数据。

17. 你想在运行在两个 GCP 区域的 Cloud Run 服务之间进行用户流量的负载均衡。Google 推荐的做法是什么？
A. 使用无服务器网络端点组（NEGs）和外部 HTTP(S) 负载均衡器。
B. 使用 Anthos Service Mesh。
C. 使用 Anthos ingress。
D. 使用多集群服务。

18. 你正在开发一个内部应用程序，并希望应用程序将日志发送到 Cloud Logging。哪个选项最适合你的需求？
A. 在实例上安装自定义 Fluentd 代理。
B. 配置 Cloud Logging 代理以包含你的应用程序日志。
C. 创建一个 cron 作业和脚本，定期调用 Cloud Logging API 发送日志。
D. 使用 Cloud Logging 代理收集自定义日志是不可能的。

19. 你想在运行在不同 Anthos GKE 集群的服务之间分发用户流量。哪个选项最适合你的需求？
A. 使用多集群服务。
B. 使用 Anthos Multi Cluster Ingress。
C. 使用在单独 GKE 集群上运行的第三方 ingress。
D. 使用网络负载均衡器。

20. 你已将应用程序的新版本部署到 Cloud Run。你看到新版本中的更改在界面中包含一个错误。你希望尽快回滚到上一个版本。选择正确的方法：
A. SSH 到容器并更改代码。
B. 使用 MANAGE TRAFFIC 功能将上一个版本的流量百分比设置为 100。
C. 撤销代码中的更改，构建新的容器镜像，并部署新版本。
D. 删除服务并使用上一个版本使用的容器镜像重新部署它。

## Mock test four

1. OS Login 可以通过在哪个级别设置元数据值来启用和禁用（选择所有适用项）？
A. VM
B. 项目
C. 组织
D. 文件夹

2. 作为安全分析师，你正在寻找为项目和资源定义细粒度的基于属性的访问控制的能力。哪项服务提供此功能？
A. Access Context Manager
B. Cloud Armor
C. Organization Policy Service
D. Data Loss Prevention

3. 存储管理员已向你提供了有关你请求的新的 Cloud Storage 存储桶的信息。你使用 Cloud Shell 设置 1 个月的保留期，但收到类似以下错误：400 cannot be set for a bucket that has a retention policy。最可能的问题是什么？
A. 存储桶仅支持以秒为单位的保留期。
B. 你没有存储桶的完全权限。
C. 存储桶已启用版本控制。
D. 存储桶使用客户管理的加密密钥。

4. 你已创建并修改了持久启动磁盘，并且需要保存状态以创建新实例。GCP 提供什么来协助你？
A. 实例模板
B. 本地 SSD
C. 公共镜像
D. 自定义镜像

5. 你希望创建一个 BigQuery 表。有哪些数据源选项可用（选择所有适用项）？
A. 上传
B. 空表
C. Google Cloud Storage
D. Cloud Spanner
E. Bigtable
F. Pub/Sub 事件

6. 你想使用现成的模板部署 GCP 资源。一位同事推荐了 Cloud Foundation Toolkit（CFT）。它提供什么（选择所有适用项）？
A. Deployment Manager 的模板
B. 安装了 ct 和 bq 的 Cloud Shell 访问
C. Terraform 的模板
D. Cloud Formation 的模板

7. 你目前在本地 VMware 上运行企业应用程序。你希望它们尽快在 Google Cloud 上运行。哪项服务可以帮助你？
A. Cloud Migration Services
B. 创建 Interconnect 网络并执行 vMotion
C. Migrate for Compute Engine
D. Cloud operations suite

8. 你被要求设计一个高可用解决方案，以安全地将本地网络连接到你的 VPC 网络。你应该使用哪项服务？
A. Cloud VPC
B. Cloud Routing
C. HA Tunneling
D. HA VPN

9. 你目前可以访问专用于托管仅你的项目虚拟机的 Compute Engine 实例，但有人要求你是否可以降低成本。在调查过程中，你注意到并非所有虚拟机都在使用所有资源。你可以深入研究什么来帮助降低成本？
A. 共同责任模型
B. 在独占租户虚拟机上超额分配 CPU
C. 迁移到 HPC 就绪的虚拟机实例
D. 在独占租户虚拟机上超额分配内存

10. 你需要将磁盘数据移动到不同的项目。正确的流程是什么？
A. 在项目 A 中备份磁盘，在项目 B 中基于备份的磁盘创建新磁盘，并将新磁盘附加到项目 B 中的实例。
B. 备份项目 A 中的 VM，将 VM 同步到项目 B，并在项目 B 中启动备份。
C. 在项目 A 中创建磁盘快照，编辑快照配置以指向项目 b，并编辑项目 B 中的实例以使用快照。
D. 在项目 A 中创建磁盘快照，在项目 B 中基于快照创建新磁盘，并将新磁盘附加到项目 B 中的实例。

11. 你的公司正在寻找一种解决方案，将轻量级数据在到达时进行转换并存储为结构化数据。哪些服务最适合此场景？
A. Cloud Storage、Cloud Run 和 BigQuery
B. Pub/Sub、Dataflow 和 Bigtable
C. Cloud Storage、BigQuery 和 Cloud Run
D. Dataflow、Pub/Sub 和 Bigtable

12. 你正在为一些开发人员创建新项目。你希望限制他们部署到特定位置的资源。你应该使用哪个 IAM 功能？
A. Workload Identity
B. 标签
C. 资源保留策略
D. 组织策略
