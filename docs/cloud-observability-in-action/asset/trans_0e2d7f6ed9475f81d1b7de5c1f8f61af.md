
# Prometheus 告警

我们在前几章中谈到了 Prometheus 并看到了它的实际应用（例如第 2 章和第 5 章）。现在在可观测埋点和后端方面有了更完整的图景，让我们看看 Prometheus 如何处理告警。

## 告警组件

在 Prometheus 的上下文中，告警包含两个不同的部分：

* *Prometheus 本身*—你定义什么是告警。这实际上意味着你使用 `PromQL` 定义条件，并且 Prometheus 会定期评估它们。一旦条件满足，Prometheus 认为该告警处于“触发”状态并将其发送到 Alertmanager API。
* *Alertmanager API*—它提供了一个 API，使客户端（通常是 Prometheus）能够发送告警并负责通知。例如，你可以定义一个策略，通过电子邮件为某些类型的告警发送通知，或实施一个策略，例如 *工作时间发送通知到 Slack；下班后发送短信*。但这并不是 Alertmanager 唯一的功能：它会去重告警，并允许你对通知进行分组和抑制，以减少告警疲劳并减轻告警风暴。

## Prometheus 与 Alertmanager 之间的交互

图 7.2 显示了 Prometheus 和 Alertmanager 如何协同处理告警。如我们在第 4 章所讨论的，抓取目标（例如你的可观测应用或 Prometheus Exporter，如 Node Exporter）以 Prometheus 暴露格式公开指标，Prometheus 会抓取这些指标（换句话说，它调用端点，默认情况下预计位于 `/metrics`）。如果你在 Prometheus 中定义了告警规则，Prometheus 会持续评估它们，并在达到定义阈值时调用 Alertmanager。Alertmanager 随后处理告警，目标是尽可能发送最少且简洁的通知。通过 Alertmanager 中配置的接收器，它将通知传递到一个或多个目的地，最终由操作员（人类）接收通知并据此采取行动。

![](OEBPS/Images/07-02.png)

图 7.2 Prometheus 和 Alertmanager

如图 7.2 所示，Prometheus 和 Alertmanager 的分离提供了灵活性和可扩展性。例如，你可以扩展或重启其中任一工具，而不会中断另一个。

### 提示

从运维角度来看，最终你希望避免告警疲劳（[https://en.wikipedia.org/wiki/Alarm\_fatigue](https://en.wikipedia.org/wiki/Alarm_fatigue)）。也就是说，你希望在确保不会遗漏重要问题的同时，将需要处理的通知降到最低。想象一下你负责一个具有多个命名空间的 Kubernetes 集群的情况。如果数据平面出现问题导致节点无法访问或重启，你只希望收到一条通知，而不是例如为集群中的每个 Pod 都收到一条通知。

现在，让我们更深入地了解告警规则和 Alertmanager 的工作原理。

## Prometheus 告警规则

从 Prometheus 开始，我们现在逐步了解如何定义和处理告警。在 Prometheus 配置中，你使用告警规则定义告警（[http://mng.bz/XNj9](http://mng.bz/XNj9)）。告警规则定义了你希望 Prometheus 评估的条件。一个具体的示例如下所示（改编自优秀的参考网站 Awesome Prometheus Alerts（[https://awesome-prometheus-alerts.grep.to/](https://awesome-prometheus-alerts.grep.to/)），它提供了大量可供使用的精选告警示例）：

```
- alert: PrometheusAllTargetsMissing    ❶
  expr: sum by (job) (up) == 0          ❷
  for: 0m                               ❸
  labels:                               ❹
    severity: critical
  annotations:                          ❺
    summary: Prometheus all targets missing {{ $labels.instance }}
    description: "A Prometheus job has no alive targets.\n
                  VALUE = {{ $value }}\n  LABELS = {{ $labels }}"
```

❶ 告警的名称。惯例是 CamelCase。
❷ 要评估的条件——Prometheus 执行的 `PromQL` 表达式
❸ 可选地，你可以指定在将告警发送到 Alertmanager 之前条件需要满足多长时间。
❹ 用于在 Alertmanager 中过滤和路由的键值元数据
❺ 使用模板化用于直接供人工消费的键值上下文（通知的内容）

虽然 `for` 字段是可选的，但你应该考虑设置它。原因在于，在云原生系统中，你会处理大量临时组件、网络连接问题以及超时或重试。你不希望一个过于敏感的告警在你这里刷屏，而实际上它只是遥测的临时问题，例如。假设我们在前面的示例中选择了 `for:` `3m`。在这种情况下，当条件首次满足时，Prometheus 最初会将告警视为待处理状态。如果条件持续 3 分钟，则告警被视为处于触发状态并发送到 Alertmanager。在前面的片段中，我们使用了 `for:` `0m`，因此告警直接进入触发状态。

关于 `annotations` 字段，你希望为基于告警接收通知的人工操作员提供丰富的上下文。考虑到通知可能在凌晨到达值班人员，而在这种情况下你最不想做的就是开始查找参考资料，浪费宝贵时间来止血，同时还在努力醒来。例如，除了我们在前一个示例中定义的 `summary` 和 `description` 字段之外，你还可以添加以下内容：

* 显示相关时间范围内时间序列的仪表板的链接
* 受影响对象（是真实客户还是金丝雀）的评估
* 指向运行手册的链接，用于已知问题和促成因素。如果你可以从手机上点击这些链接，而无需登录例如八个系统即可让它们呈现出来，那么你就做得很好。

## 使用告警规则配置 Prometheus

现在我们知道了告警规则是什么以及如何定义它们，让我们看看如何设置 Prometheus 以包含告警规则并建立与 Alertmanager 的连接。这在 Prometheus 配置文件 `prometheus.yml` 中如下所示完成：

```
global:
  scrape_interval: 30s
  external_labels:       ❶
    environment: dev
    region: us-west-1
rule_files:
  - alert.rules          ❷
alerting:
  alertmanagers:         ❸
  - static_configs:
    - targets: ["alertmanager.local:9093"]
scrape_configs:
  ...
```

❶ 添加外部标签以提供 Prometheus 实例的上下文
❷ 指向定义告警规则的文件
❸ 配置 Prometheus 应使用的 Alertmanager

## Alertmanager

现在让我们转向 Alertmanager（[http://mng.bz/yQ7p](http://mng.bz/yQ7p)）。首先，我们来看一下 Alertmanager 如何处理告警。这是在所谓的通知管道中完成的，包含以下阶段：

### 通知管道阶段

1. *抑制*—如果其他（相关）告警已在触发，则抑制告警的通知。例如，考虑前述情况，当你已经收到集群数据平面问题的通知时，你不希望收到 Pod 级别的通知。
2. *静默*—以主动的方式临时抑制通知。例如，如果你正在升级 Kubernetes 集群中的节点，你事先就知道节点会暂时宕机，因此你不希望为此收到通知。
3. *路由*—决定谁接收通知。
4. *分组和节流*—确保每个事件只收到一条通知。
5. *通知*—使用所谓的接收器，定义通知应如何传递，例如通过电子邮件、Slack、PagerDuty 或亚马逊简单通知服务（SNS；[https://aws.amazon.com/sns/](https://aws.amazon.com/sns/)）。

