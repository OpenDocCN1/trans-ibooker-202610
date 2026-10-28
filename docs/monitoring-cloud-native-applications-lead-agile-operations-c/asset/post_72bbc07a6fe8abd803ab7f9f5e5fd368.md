
![](img/6646c41cea18bb4f7d3c2c6513a4d992_242_0.png)

Figure 6-68. Setting alert notifications

Grafana has a scheduler and query execution engine where alert rules are evaluated. It supports only a limited number of data sources like Prometheus, InfluxDB, Elasticsearch, and so forth. In case you have deployed Grafana in a highly available setup, all the servers will execute the alerts, but alert notifications will be deduped, sending out a single notification only.

## State History

The changes in the state of an alert are tracked in an internal table in Grafana’s database. As seen in Figure 6-69, the last 50 state changes from OK to ALERTING are captured with date and time. You can also clear the state history by clicking the Clear History button. The state changes are marked in the graph panel as annotations.

As shown in Figure 6-70, there are two more sections where you can find more details on the alert which is firing. One section shows the object which is satisfying the alert rule. In Figure 6-70, it’s the handler="/metrics" which has a value greater than the configured alert threshold of 0.5. The second section is for logs, which shows that the value for the object handler="/metrics" is higher than the threshold limit of 0.5.

![](img/6646c41cea18bb4f7d3c2c6513a4d992_244_0.png)

Figure 6-70. Testing the configured alert rule

Once the alert is successfully configured, it will appear under the Alert Rules tab in the Alerting section as seen in Figure 6-71.

![](img/6646c41cea18bb4f7d3c2c6513a4d992_245_0.png)

Figure 6-71. Alert rule firing

## Adding a Notification Channel

A notification channel is the medium where you want to be notified instantly whenever your monitoring system generates an alert. In order to receive notifications in your favorite notification channel, integrate it first with Grafana from the Notification channels tab. See Figure 6-72.

![](img/6646c41cea18bb4f7d3c2c6513a4d992_245_1.png)

Figure 6-72. Adding a notification channel

- Grafana (cont.)
  - creating, 233
  - defining, 232
  - notification channel, 238, 239
  - setting, 234, 235
  - state, 235, 236
  - test rule, 236–238
  - creating dashboard
    - panel editor, 203–211
    - panels, 202, 203
  - definition, 187
  - features, 187
  - installation, 188–192, 195
  - visualization
    - bar gauge, 212
    - display settings, 212–215
    - gauge, 212
    - Stat, 211
- **H**
  - Heatmaps, 83, 187
- **I, J**
  - InfluxDB
    - Alerting
      - configure, 175
      - configure checks, 176–178
      - notification endpoint, 182
      - setting thresholds, 179–181
    - bucket, 154
    - data collection
      - scraper, 154, 156
      - Telegraf, 156, 158, 159, 161–163, 165, 166
    - data storage, 168
    - installation, 134, 136, 137, 139, 140, 142
    - members, 148–153
    - organization, 142, 145
      - creating, 143, 144
      - delete, 148
      - rename, 145–147
    - visualization, 182–185
- **K**
  - Key performance indicators (KPI), 14
  - Kubernetes, 22
    - API server, 48
    - architecture, 23
    - Borg, 22
    - cAdvisor, 49, 50
    - cluster, 47
    - CNCF project, 22
    - command, 47
    - components, 23, 24
    - ConfigMap, 103
    - containers, 22
    - internal metrics
      - namespaces, 52
      - nodes, 52
      - pods, 53
      - summary usage, 52
    - kube-state-metrics, 50
    - list of pods, 47
    - Master node, 23
    - master nodes, 51
    - metrics server, 50
    - microservices-based application, 22
    - monitoring, 46
    - namespaces, 102
    - observability, 46
    - Prometheus, 45, 46
    - purpose, 22
    - sources of metrics, 47
- Modern monitoring system (cont.)
  - OpenTelemetry, 63, 64
    - agent, 66, 67
    - collector, 65
    - collector architecture, 65, 66
    - export, 64
    - gateway, 66
- Monitoring domains
  - applications, 13
  - business KPI, 14
  - classification, 11, 12
  - cloud infrastructure, 12
  - containers, 13
  - services, 13
  - user experience, 13
    - Clickstream monitoring, 14
    - Synthetic monitoring, 14
- Monitoring
  - capacity usage, 8
  - definition, 8
  - DevOps
    - evolution, 9, 10
    - five pillars, 10, 11
    - SLOs, 10
    - SREs, 10
  - performance trends, 8
  - reactive vs. proactive, 15
- Monitoring systems
  - AIOps, 21
  - definition, 16
  - features
    - aggregation, 18
    - auto-notification, 19
    - collecting data, 18
    - discovery, 19
  - modern-day systems, 19
  - traditional monitoring, 19
  - types, 16
  - use cases
    - detecting problems, 17
    - ensuring availability, 17
    - measuring performance, 17
- **O**
  - Observability
    - definition, 25
    - events, 26, 27, 44, 45
    - logs, 27
      - RPC, 28
      - root cause analysis, 27
    - metrics, 26
    - objectives, 26
    - traces, 26
      - RPC, 28
  - OpenTelemetry collector, 64, 65, 67
  - OpenTelemetry
    - Protocol (OTLP), 64
- **P, Q**
  - Prometheus, 99
    - alerting, 129
      - grouping, 129
      - inhibition, 130
      - routing, 129
      - silences, 131
    - architecture, 110
      - Alertmanager, 129
    - Borgmon, 99
    - data collection, 110, 111
    - data storage, 121
    - exposition, 111–114
    - service discovery, 117, 118
