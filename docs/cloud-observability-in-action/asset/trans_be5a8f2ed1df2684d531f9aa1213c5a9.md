
对于指标，Kubernetes 使用 Prometheus 暴露格式（[http://mng.bz/x4V8](http://mng.bz/x4V8)）来发出指标，该格式定义在指标管道（[http://mng.bz/Aopp](http://mng.bz/Aopp)）中，后者是控制平面的一部分。与日志不同，指标的定义和管理是集中的。你可以直接使用 `kubectl` `get` `--raw` 命令查询这些原始指标，格式为 Prometheus 暴露格式，用于调试目的。以下代码提供了一个示例（已编辑以适应）：

```
$ kubectl get --raw /metrics | grep --color=auto apiserver | head -5       ❶
# HELP apiserver_admission_controller_admission_duration_seconds [ALPHA]
  Admission controller latency histogram in seconds, identified by name
  and broken out for each operation and API resource and type (validate
  or admit).
# TYPE apiserver_admission_controller_admission_duration_seconds histogram ❷
apiserver_admission_controller_admission_duration_seconds_bucket{
name="CertificateApproval",operation="UPDATE",rejected="false",
type="validate",le="0.005"} 9.729562e+06
apiserver_admission_controller_admission_duration_seconds_bucket{
name="CertificateApproval",operation="UPDATE",rejected="false",
type="validate",le="0.025"} 9.729568e+06
apiserver_admission_controller_admission_duration_seconds_bucket{
name="CertificateApproval",operation="UPDATE",rejected="false",
type="validate",le="0.1"} 9.729568e+06
```

❶ 访问 `/metrics` 端点以获取原始指标，使用 `grep` 过滤包含 `apiserver` 的指标，并使用 `head` 限制为五行。

❷ 这里显示的指标 `apiserver_admission_controller_admission_duration_seconds` 是控制平面组件（准入控制器）的一种直方图类型。

Traces 是 Kubernetes 中最近添加的信号类型。它使用 OpenTelemetry（[http://mng.bz/Zq6m](http://mng.bz/Zq6m)）来捕获并暴露（截至撰写时）API 服务器的跟踪。

### 3.2.4 无服务器计算

本节中，我们将从无服务器计算服务提供的角度来看信号源。这些无服务器计算环境通常从运营角度来看比自行管理服务器更可取；然而，在某些情况下你无法使用它们。其中一些情况包括出于性能原因需要控制某些低级设置的使用案例，例如 Linux 内核模块或硬件配置。另一类使用案例来自监管要求，如果金融行业或医疗行业工作负载所需的某些认证不可用。

如果可以的话，你应该强烈考虑为你的工作负载使用无服务器计算。让我们先讨论无服务器计算的一般特征以及这些环境对可观测性方面的 implications。

#### 无（访问）服务器

按定义，*serverless* 意味着你不处理服务器——也就是说，你不提供或维护服务器。自然地，这些服务器仍然存在于你的提供商账户中，但现在维护它们是他们的责任。一个含义是，你通常没有对服务器的 `ssh` 访问权限，因此，前一节中与 `journalctl` 相关的许多陈述不适用。相比之下，你处理的是允许你消费日志或指标的 API，以及允许在浏览器中查看相同内容的提供商控制台仪表板。

#### 无需修补

从运营角度来看，你外包给无服务器计算提供商的维护的重要部分是安全补丁，通常在操作系统和（语言）运行时级别。这意味着，虽然你仍然希望跟踪漏洞（尤其是在应用级别），但除测试外，你这边所需的工作更少。

#### 自动扩缩容

无服务器计算中的一个突出期望是自动扩缩容。这意味着垂直（使用 CPU/RAM 更多或更少的虚拟机）或水平（添加或移除相同的机器），以改变计算容量以适应变化的负载。无服务器计算让你处理自动扩缩容而无需担心缩放模型的细节，这反过来减少了你需要摄取的指标数量。

#### 按需付费

通常，无服务器采用按需付费模式。换句话说，如果你不使用容量，理论上你就不必为其付费。对此有例外情况（例如，像 Kubernetes 这样的编排器的固定费用），计费粒度也可能是一个因素。这里的含义是，你需要确保以自动化方式跟踪成本（包括警报），尤其是在考虑自动扩缩容的情况下。

从可观测性角度来看，无服务器环境通常意味着所需的工作更少，因为很大一部分运营工作已外包给提供商。潜在的缺点是缺乏可见性——如果操作系统或虚拟机信号未明确公开，你就无法访问它。

#### 容器与函数

无服务器计算服务大致分为两种类型：以容器为中心和函数即服务（FaaS）。让我们快速了解一下主要云提供商在此领域提供的服务。

Amazon Web Services (AWS) 中的以容器为中心的无服务器服务是 AWS Fargate，你可以在两种容器编排器的上下文中使用它作为数据平面，要么在 Amazon ECS on AWS Fargate（[http://mng.bz/Rxrj](http://mng.bz/Rxrj)）中，要么在 Amazon EKS on AWS Fargate（[http://mng.bz/2DR8](http://mng.bz/2DR8)）中。根据你使用的编排器，可用的信号不同。例如，EKS on Fargate 提供基于 Fluent Bit 的日志路由解决方案（[http://mng.bz/D4Gy](http://mng.bz/D4Gy)）以及通过 CloudWatch 的指标（[http://mng.bz/1qZ1](http://mng.bz/1qZ1)），而 ECS on Fargate 提供通过 CloudWatch 的监控（[http://mng.bz/PzBY](http://mng.bz/PzBY)）以及 Prometheus 格式的指标。

AWS Lambda（[http://mng.bz/JgWK](http://mng.bz/JgWK)）是 FaaS 无服务器计算服务。如图 3.4 所示，它提供了一种统一且全面的可观测性（[http://mng.bz/j129](http://mng.bz/j129)）方法，其中包括 AWS X-Ray（[http://mng.bz/wv12](http://mng.bz/wv12)）。在此上下文中，你还应考虑如何节省 Amazon CloudWatch Logs 成本（[http://mng.bz/qrVJ](http://mng.bz/qrVJ)）。

![](OEBPS/Images/03-04.png)

图 3.4 控制台中的 AWS Lambda 监控

在 Microsoft 的云中，有许多无服务器计算服务，最突出的是 Azure Container Instances（[http://mng.bz/7Dge](http://mng.bz/7Dge)），它具有 Azure Monitor（[http://mng.bz/mVl4](http://mng.bz/mVl4)）支持，涵盖 CPU、内存和网络使用以及全面的日志管理。此外，还有具有集成可观测性（[http://mng.bz/6DaA](http://mng.bz/6DaA)）的 Azure Container Apps（[http://mng.bz/5wMa](http://mng.bz/5wMa)）无服务器服务。Azure Functions（[http://mng.bz/o1dp](http://mng.bz/o1dp)）是 FaaS 服务。它与 Azure Application Insights（[http://mng.bz/nWB2](http://mng.bz/nWB2)）集成，用于日志和指标，以及基于 OpenTelemetry 的跟踪支持。

Google Cloud Run（[https://cloud.google.com/run/](https://cloud.google.com/run/)）是 Google 云的以容器为中心的无服务器服务，针对无状态服务进行了优化。它附带 Google Cloud 的 operations suite（[https://cloud.google.com/products/operations](https://cloud.google.com/products/operations)—formerly Stackdriver）集成，支持日志、指标、跟踪和配置文件。Google Cloud Functions（[https://cloud.google.com/functions/](https://cloud.google.com/functions/)）是 Google 的 FaaS 服务，同样使用 Google Cloud 的 operations suite 进行可观测性。至此，我们结束了与计算相关的源，并转向各种形式的状态。

## 3.3 与存储相关的源
