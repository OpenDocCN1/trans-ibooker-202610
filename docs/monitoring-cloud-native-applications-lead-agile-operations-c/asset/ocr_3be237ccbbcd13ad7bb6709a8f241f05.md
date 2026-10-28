![](bbox=[0.22, 0.13, 0.78, 0.34])

![](bbox=[0.22, 0.38, 0.78, 0.58])

# Figure 2-1. Metrics and events

Events are another important sub-pillar which is usually clubbed with metrics but should definitely be a part of any sound observability strategy. The difference between metrics and events is that metrics are collected at a regular time interval whereas events are a record of significant occurrences from the same system under observation. Events such as alerts getting triggered, deployment failures, and transaction completions are all important happenings taking place at irregular time intervals. Events contain metadata to sharpen the context and provides the ability to perform real-time fine-grained analysis.

Logs are detailed journals of everything that has happened within a system in a structured text format and are crucial for performing root cause analysis. They can help in recreating the past, as these are comprehensive records providing high-fidelity data and detailed context around an occurrence. Logs are either printed out as events occur or stored for future analysis. While there are standard schemas for logs (as in the case of