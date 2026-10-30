Figure 14-7. Slack’s CI Service Traces dashboard displaying queries available to view different pieces of CI interactions

In Figure 14-8, you can see an example of a query looking at a high level at a rate, error, and duration query that responding teams might use.

# Checkpoint Traces

| VISUALIZE | WHERE | GROUP BY |
|---|---|---|
| HEATMAP(duration_ms) | hostname starts-with checkpoint | method |
| P50(duration_ms) |  | service_name |
| P95(duration_ms) |  |  |
| P99(duration_ms) |  |  |
| COUNT_DISTINCT(error) |  |  |
| COUNT |  |  |

Figure 14-8. This sample drill-down query approximates a rate, error, and duration (RED) dashboard with visualizations by grouping individual methods between services in Checkpoint

With actionable alerting embedded, now we can understand what changed.

# Understanding What Changed

Let’s explore another case study of an August 2021 incident that combines a few of the preceding ideas. At Slack, we use the incident command