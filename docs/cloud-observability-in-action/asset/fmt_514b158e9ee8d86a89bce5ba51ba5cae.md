
In general (ignoring the details of the packaging and scheduling, such as a `Kubernetes` pod versus a `Lambda` function), cloud-native systems have the following characteristics:

*   *They are distributed in nature.* Most of the communication is not in process—and maybe not even on the same machine (e.g., using inter-process communication \[`IPC`\] mechanisms)—but takes place via the network.

*   *Due to the distributed nature, not only the* what *but also the* where *is crucial.* Think of a number of microservices along the request path serving a specific user. You want to be able to understand what’s going on end to end and also be able to drill down into each of the services.

*   *The volatility of the services is higher compared to a monolith running on, say, a `VM`.* `Pods` (and, with it, their IP addresses) come and go, new versions of a `Lambda` function might be pushed out every hour, and so on.

Let’s now look at how observability can help address the challenges found in and with cloud-native systems. With cloud native systems, you don’t want to fly blind; having an automated way to collect all relevant signals and use them as input for decisions—that is, actionable insight—is the ideal.

### 1.5.1 Return on investment

To understand the right level of investment (i.e., time and money) into observability, you need to know about the costs first:

*   *Instrumentation costs*—These include developers’ time, which is an ongoing cost.

*   *Signal retention*—These costs (e.g., log storage) can be one-off but usually fall under the pay-as-you-go model.

*   *Overhead of agents and instrumentation*—This manifests in compute and memory resource usage on top of what the service itself uses. Costs should be in the low single-digit range of `CPU` and `RAM` utilization. For example, the `AWS` `OpenTelemetry Collector` ([http://mng.bz/N2J1](http://mng.bz/N2J1)) provides a report on the footprint across different signal types.

*   *Network usage*—Costs of this type, such as egress traffic, may be a significant cost driver for observability solutions.

The return on the investment into observability is harder to calculate. Determining your return depends mainly on how clear the goals are. We will discuss the goals of different roles shortly. For example, you can measure the mean time to recovery (`MTTR`; [https://www.atlassian.com/incident-management/kpis/common-metrics](https://www.atlassian.com/incident-management/kpis/common-metrics)) for a given service before and after instrumentation. But there are also many human-related factors (e.g., on-call stress or work–life balance) that count toward the gain. You can read more about the topic in “Return on Investment Driven Observability” ([https://arxiv.org/abs/2303.13402](https://arxiv.org/abs/2303.13402)), a short paper I published on the topic.

### 1.5.2 Signal correlation

Different signal types are usually good for different tasks. Also, a single signal type is typically unable to answer all the questions you might have. We will discuss signal correlation in detail in chapter 11; however, I’d like to provide you with a quick motivational example here.

Let’s say you are on call and get paged (in the middle of the night). You fire up your laptop and look at a `Grafana` dashboard linked in the alert you received. From the dashboard, which, say, plots `Prometheus` metrics, you can gather that certain services in your microservices app have an increased error rate in the past 20 minutes. You have a hunch that this may be related to the recent upgrade of a service. You use the trace ID from an exemplar ([http://mng.bz/D4Jw](http://mng.bz/D4Jw)) embedded in the `Prometheus` metrics as an entry point to have a closer look at the involved services, using the distributed tracing tool `Jaeger`. In `Jaeger`, you see the traces duration distribution and error codes and drill down into the logs of the service to verify your hypothesis.

In this scenario, you correlated the different signal types (metric to traces to logs) and were able to confirm your hypothesis quickly and effectively. A simple form of correlation is a time-based one, which is something you get in many frontends, such as `Grafana`, without additional effort out of the box. This means you use a time period to query different backends to manually find signals that tell you the whole story.

### 1.5.3 Portability

Avoid lock-in by choosing open standards and open source. This ensures you can use the same mechanisms and tooling in different environments (e.g., cloud providers or on-premises infrastructure) without having to change much of your setup. For example, if you instrument your applications using `OpenTelemetry`, then you can switch from one destination to another without having to change your application or agents.

## Summary

*   Cloud-native systems, such as `Kubernetes`, produce a large volume of telemetry signals.

*   Sources of telemetry signals can be your code, APIs (e.g., an `AWS` service), a database, and so on.

*   The term *telemetry* refers to the process of collecting signals from sources, processing them via agents, and ingesting them into observability destinations, where they can be consumed by humans and machines.

*   In general, *observability* means getting actionable insights from signals to understand and influence the system under observation.

*   There are many use cases for observability, from reducing `MTTR` to cost optimizations to increasing developer productivity.

*   Observability is relevant to different roles (developers, DevOps, nontechnical roles, etc.) to perform their typical tasks.

*   We had a look at a concrete example setup of a cloud-native app (a containerized microservices app in `Kubernetes`), its signals, and what observability could mean in this context.

*   Challenges of cloud-native systems include their distributed nature and the volatility of the involved components.

*   Observability can help address the challenges found in cloud-native systems by allowing you not to fly blind.

*   It is important to understand the costs and return on investment of observability.

*   Signal correlation is important, as it enables you to combine different signal types to answer all questions you may have about a system.

# 2 Signal types

This chapter covers

*   What different signal types are

*   When and how to use a certain signal type

*   The costs and benefits of each signal type

*   How to figure out which signals are relevant

*   Determining signal baselines

In the context of observability, signals play a central role: this is the intel we’re basing our decisions on. On a high level, we can differentiate between signals numerical in nature (e.g., the number of service invocations) and signals that carry textual payload that requires human interpretation (e.g., a log line).

As discussed in chapter 1, the system under observation, or just *system* (e.g., a `Kubernetes` application or a serverless app based on `Lambda` functions), emits signals from various sources. You, as a human, or equally a piece of software, then use these signals to understand the system and make decisions to influence it. There is a one-to-many relation between the system and sources. Your system typically involves compute, storage, and network, each of these representing a signal source. We will cover the sources in detail in chapter 3.

In terms of signals (and sources), we can differentiate between two cases:

*   Sources, such as code, that you own and can instrument yourself (that is, decide where and what kind of signals to emit).

*   Sources you don’t own, meaning any signals will be predefined. Your task is limited to selecting which signals to consume, in this case. This might be due to several reasons:

    *   Code that is a dependency of your code (library, package, module, etc.).

    *   The component’s source code is not available (e.g., a binary file).
