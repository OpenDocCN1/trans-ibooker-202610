
workflows. I hope by sharing Slack’s approach to these problems, you can reuse these patterns for your own internal development.

## Understanding Context Through Tooling

CI operates in a complex, distributed system; multiple small changes can be additive in their effect on the CI user experience. Multiple teams traditionally operated in silos to debug performance and resiliency issues in order to better serve their customers (e.g., backend, frontend, and middleware). However, for customers of CI who are doing development and then running tests, the user experience is best represented by the worst-performing or flakiest tests. A test is considered flaky when running the same code has a different result. Understanding context is critical for identifying bottlenecks.

For the rest of this section, I will walk through a case study of Slack teams working together to understand context from the problem of flaky tests. Slack teams were able to significantly reduce this problem for their internal customers by focusing on a few key tenets:

- Instrumenting the test platform with traces to capture previously underexplored runtime variables
- Shipping small, observability-driven feedback loops to explore interesting dimensionality
- Running reversible experiments on dimensions that correlated with flaky test configurations

Developer frustration across Slack engineering was increasing because of flaky end-to-end test runs in 2020. Test turnaround time (p95) was consistently above 30 minutes for a single commit (the time between an engineer pushing a commit to GitHub and all test executions returning). During this period, most of Slack’s code testing was driven by end-to-end tests before an engineer merged their code to the mainline. Many end-to-end test suites had an average suite execution flake rate of nearly 15%.

Cumulatively, these flaky test executions peaked at 100,000 weekly hours of compute time on discarded test executions.

By mid-2020, the combination of these metrics led to automation teams across Slack sharing a daily 30-minute triage session to dig into specific test issues. Automation team leads hesitated to introduce any additional variance to the way Slack used Cypress, an end-to-end testing platform. The belief was that flakiness was from the test code itself. Yet no great progress was made in verifying or ruling out that belief.

In late 2020, observability through tracing had shown great promise and impact in identifying infrastructure bottlenecks in other internal tooling. Internal tooling and automation teams worked to add tracing for a few runtime parameters and spans in Cypress.

Within days of instrumentation, multiple dimensions appeared very correlated with test suites that had higher flake rates. Engineers from these teams looked at this instrumentation and discovered that users and test suite owners of the test platform had drastically different configurations. During this discovery process, additional telemetry was added to the Docker runtime to add additional color to some flakes. Empowered with data, these engineers experimented to place better defaults for the platform and to place guardrails for flaky configurations. After these initial adjustments, test suite flake rates decreased significantly for many users (suites went from 15% to under 0.5% flake rates).

Because of the disparate nature of the runtime between end-to-end test suites that no central team had visibility into, context gathering became a critical piece between suites to identify specific dimensions that caused flakiness.

## NOTE

> For more on how Slack evolved its testing strategy and culture of safety, see the Slack blog post [“Balancing Safety and Velocity in CI/CD at Slack”](https://slack.engineering/balancing-safety-and-velocity-in-ci-cd-at-slack/). Slack describes how engineers initiated a project to transform testing pipelines and de-emphasize end-to-end testing for code safety. This drastically reduced user-facing flakiness and increased developer velocity in 2021.

## Test Execution Flaky Runtime with Attribution (Quarterly View)

![](img/aaaa2d27e08e754554d501192f304d8f_248_0.png)

Figure 14-5. Time spent on flaking test runs between major classes of test runs for web app. The yellow bar shows flaky test executions from the Cypress platform tests.

With this shared understanding of context through tooling, Slack’s next step was to embed actionable workflows through alerting.

## Embedding Actionable Alerting

Slack integrates Slack into how engineers handle their development and triage issues in the infrastructure. Observability through tracing plays a critical role in helping engineers do their job by directing people in Slack messages and configured dashboards.

Let’s examine a case study around test execution. A single test suite execution might be understood in different ways depending on code, infrastructure, or features in the product. Each team or platform might have multiple SLOs for various parts of a test execution. Here are a few examples:

- Test suite owner or platform teams might care about flakiness, reliability, or memory usage.
- Test infrastructure teams might care about performance and reliability of specific operations (like Docker ops or cost per test).
- Deployment owners might care about what was tested or upcoming hotfixes coming through CI.
- Internal tooling teams might care about throughput of test result processing through CI.

The prompt for identifying an issue might be anomaly detection alerts for a high-level business metric or a specific issue that’s suite based (e.g., in Figure 14-6). The link to our observability tool might direct the user to a collection of views available based on the test_suite dimension.

## CI Service Traces

This describes our current traces within CI. Each can be queried with our canonical dimension set described https://paper.dropbox.com/doc/4AsZ6B3C6E1H9T7Ss

Frank Chen last edited Jan 10, 2022

Last 28 days

Settings

One-Column

Multi-Column

### Checkpoint Traces

Jan 27 2022, 5:07:43 PM - Feb 24 2022, 5:07:42 PM (Granularity: 2 hr)

### Cibot Traces

Jan 27 2022, 5:07:43 PM - Feb 24 2022, 5:07:42 PM (Granularity: 1 hr)

### Cypress Traces

Jan 27 2022, 5:07:43 PM - Feb 24 2022, 5:07:42 PM (Granularity: 1 hr)

Describe this Query

Tools

Figure 14-7. Slack’s CI Service Traces dashboard displaying queries available to view different pieces of CI interactions

In Figure 14-8, you can see an example of a query looking at a high level at a rate, error, and duration query that responding teams might use.

### Checkpoint Traces

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

## Understanding What Changed

Let’s explore another case study of an August 2021 incident that combines a few of the preceding ideas. At Slack, we use the incident command system to handle incidents when an outage or service degradation is detected. The incident response begins with a new public Slack channel, and an incident commander adds responders from multiple teams to coordinate response and remediation of an outage.

In this situation, multiple users noted being blocked after seeing high rates of failures due to out-of-memory errors (OOMs) on test suite execution for backend unit and integration test suites. Each of these backend unit test suites may run tens of thousands of tests, and individually, test suite executions were occasionally passing.

Early in the incident, a responder found an anomaly from the previous day noting a higher flake rate (see Figure 14-9 for types of anomalies), potentially pointing to something that changed around that time. During the incident, responders looked at test case traces from backend tests to look at jumps in memory footprint for test cases. We were able to see multiple jumps in memory usage at p50, p95, and p99 over the last months, with fidelity down to the hour on what had changed.
