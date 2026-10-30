
# Prometheus Alerting

We talked about Prometheus and saw it in action in previous chapters (e.g., in chapters 2 and 5). Now that we have a more complete picture in terms of instrumentation and backends, let’s see how Prometheus handles alerting.

## Alerting Components

In the context of Prometheus, alerting comprises two distinct parts:

* *Prometheus itself*—You define what an alert is. This effectively means you define conditions using `PromQL`, and Prometheus evaluates them periodically. Once a condition is met, Prometheus considers this to be the alert “firing” and sends it to the Alertmanager API.
* *Alertmanager API*—This provides an API that enables clients (typically Prometheus) to send in alerts and takes care of the notifications. For example, you could define a policy that sends a notification for certain types of alerts via email or implement a policy such as *during work hours, send the notification to Slack; after work, send a text message*. But that’s not the only thing the Alertmanager does: it deduplicates alerts and allows you to group and suppress notifications to reduce alert fatigue and lessen alert storms.

## Interaction Between Prometheus and Alertmanager

Figure 7.2 shows how Prometheus and Alertmanager work together to handle alerts. As we discussed in chapter 4, the scrape targets (e.g., your instrumented applications or Prometheus exporters, such as the Node exporter) expose metrics in the Prometheus exposition format, which Prometheus scrapes (in other words, it calls the endpoints, which are by default expected to be at `/metrics`). If you have alerting rules defined in Prometheus, Prometheus continuously evaluates them and calls the Alertmanager in case the defined threshold is reached. The Alertmanager, in turn, processes alerts with the goal of sending out as few and concise notifications as possible. Via the configured receivers in the Alertmanager, it delivers a notification to one or more destinations, where the operator (a human) finally consumes the notification and acts upon it accordingly.

![](OEBPS/Images/07-02.png)

Figure 7.2 Prometheus and the Alertmanager

The separation of Prometheus and the Alertmanager, as shown in figure 7.2, allows for flexibility and scalability. For example, you can scale or restart either of the tools without interrupting the other.

### Tip

From an operations perspective, at the end of the day, you want to avoid alert fatigue ([https://en.wikipedia.org/wiki/Alarm\_fatigue](https://en.wikipedia.org/wiki/Alarm_fatigue)). That is, you want to minimize the notifications to act on while making sure you don’t miss something substantial. Imagine the case in which you’re responsible for a Kubernetes cluster with multiple namespaces. If there are data plane problems with the nodes not being reachable or rebooting, you only want to get one notification, rather than, say, one notification for every single pod in the cluster.

Now, let’s dive deeper into how alerting rules and the Alertmanager work.

## Prometheus Alerting Rules

Starting with Prometheus, we now work our way through defining and handling alerts. In the Prometheus configuration, you define the alerts using alerting rules ([http://mng.bz/XNj9](http://mng.bz/XNj9)). An alerting rule defines the condition you want Prometheus to evaluate. A concrete example looks as follows (adapted from the excellent reference site Awesome Prometheus Alerts ([https://awesome-prometheus-alerts.grep.to/](https://awesome-prometheus-alerts.grep.to/)), which provides a large collection of curated alert examples you can use):

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

❶ The name of the alert. The convention is CamelCase.
❷ The condition to evaluate—a `PromQL` expression Prometheus executes
❸ Optionally, you can specify for how long the condition needs to be met before sending the alert to the Alertmanager.
❹ Key–value metadata for filtering and routing in the Alertmanager
❺ Key–value context for direct human consumption (content of the notification) using templating

While the `for` field is optional, you should consider setting it. The reason is that, in cloud-native systems, you’re dealing with a lot of ephemeral components, network connectivity issues, and timeouts or retries. You don’t want a trigger-happy alert spamming you while, really, it was just a temporary issue with the telemetry, for example. Let’s say we would have chosen `for:` `3m` in the preceding example. In this case, Prometheus initially considers the alert in the pending state when the condition is met for the first time. The alert is considered to be in the firing state and sent to the Alertmanager if the condition persists for 3 minutes. In the preceding snippet, we used `for:` `0m`, and because of this, the alert went directly to firing.

Concerning the `annotations` field, you want to provide rich context for the human operator receiving a notification based on the alert. Consider that the notification may reach the person on call at an early hour, and the last thing you want in this situation is to start digging for references, wasting valuable time to stop the bleeding, while still trying to wake up. For example, you could, in addition to the `summary` and `description` fields we defined in the preceding example, add the following:

* A link to a dashboard that shows the time series for the relevant time frame
* An assessment of who or what is impacted (whether these are real customers or canaries)
* A link to a runbook, for known issues and contributing factors. If you can follow these links from your phone without having to log in to, say, eight systems to get them to render, you’ve done a good job.

## Configuring Prometheus with Alerting Rules

Now that we know what alerting rules are and how to define them, let’s see how to set up Prometheus to include the alerting rules and establish the connection to an Alertmanager. This is done as follows in the Prometheus configuration file `prometheus.yml`:

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

❶ Adds external labels to provide context for the Prometheus instance
❷ Points to the file where the alerting rules are defined
❸ Configures the Alertmanager Prometheus should use

## Alertmanager

Let’s now move on to the Alertmanager ([http://mng.bz/yQ7p](http://mng.bz/yQ7p)). First, we have a look at how the Alertmanager processes alerts. This is done in the so-called notification pipeline, consisting of the following phases:

### Notification Pipeline Phases

1. *Inhibition*—Suppress notifications for alerts if other (related) alerts are already firing. For example, think of the preceding case, where you don’t want pod-level notifications when you’re already notified of a cluster data plane issue.
2. *Silencing*—Suppress notifications ad hoc, in a proactive manner. For example, if you’re upgrading nodes in an Kubernetes cluster, you already know in advance that nodes will go down temporarily, so you don’t want to get a notification for this.
3. *Routing*—Decide who gets a notification.
4. *Grouping and throttling*—Ensure you get one notification per event.
5. *Notification*—Using so-called receivers, define how the notification should be delivered, such as via email, Slack, PagerDuty, or Amazon Simple Notification Service (SNS; [https://aws.amazon.com/sns/](https://aws.amazon.com/sns/)).
