
Then, there are offerings from dedicated observability vendors, such as Splunk, Datadog, Dynatrace, Honeycomb, New Relic, Lighstep by ServiceNow, AppDynamics by Cisco, and Sumo Logic, which typically combine frontends and backends, providing an integrated experience. In those cases, you can’t use the frontend (UI) separately from the backend. There are also mixed cases in the open source space, like CNCF `Jaeger` (which sports an ingestion pipeline but requires an additional storage system). We will cover these all-in-ones or unified destinations in chapter 6.

Another special case is cloud-provider-native offerings, especially Amazon CloudWatch, Azure Monitor (including Application Insights), and the Google Cloud operations suite, which cover a wide range of features and provide integrations with different backends and frontends.

Note Like I pointed out in chapter 1, regarding why I am using the logs, metrics, and traces boundaries to present topics, I am making the frontend–backend distinction mostly on a conceptual level. This should aid in your understanding; however, by now, you know that the reality is much messier and more complicated.

Now, let’s get into it. In this chapter we will
*   Have a look at some commonalities and general backend terminology
*   Review options for and usage of logs backends
*   Review options for and usage of metrics backends
*   Review options for and usage of traces backends
*   Discuss columnar data stores
*   Discuss how to go about selecting a backend

Ready? Let’s dive in!

## 5.1 Backend destination terminology

Backends are the source of truth for all your observability questions, be that a human asking them (ad hoc, based on a hunch) or a piece of code (think an alert that queries a backend to evaluate if a certain condition is met). Before we get into the different types of backends and their pros and cons, we will first step back a bit and look at their common characteristics along with some basic terminology we need for the rest of the chapter and book.

In general, there are three main parts you want to consider with backends, whether that is concerning performance or costs (see section 5.6 for more):
*   `Ingestion`—How the signals arrive at the backend. Various protocols and formats exist, such as OpenTelemetry’s `OTLP` or the `Prometheus` formats.
*   `Storage`—Usually, there’s the expectation that the signals ingested are stored for a longer time period (weeks, months, or even years), allowing for historical query and data analytics. But this doesn’t always have to be the case, as pure in-memory solutions exist (e.g., CNCF `Pixie`) and have their uses, such as live cluster debugging, keeping only hours of (volatile) data around.
*   `Query`—The more exact and common term we use for *asking a backend a question* is to *query the backend*. What exactly *query* means can differ from backend to backend and can range from a declarative style, such as `SQL`, to an `API` call, such as is the case with many tracing backends. You will also often come across domain-specific languages (`DSLs`), such as `PromQL`, and you’ll see several examples of these DSLs in use throughout this chapter. With this in mind, let’s move on to the first, and probably most widely adopted, kind of backend, for all sorts of logs.

## 5.2 Backend destinations for logs

In this section, we will take a cursory look at backend destinations that you can use to ingest logs and query them. As we discussed in chapter 2, logs are timestamped, typically capturing events; have a (hopefully structured) payload; and are primarily meant for human consumption. The sheer number of different log types and formats makes it sometimes difficult to figure out what the optimal backend is. Usually, it depends heavily on the use case and your preferences, such as open source and open standards first, or you might prefer a managed offering over a DIY approach (or the other way round). Having said that, and of course this also applies to the rest of the backends for metrics and distributed traces, let’s see what we have here.

### 5.2.1 Cloud providers

Every cloud provider has, at the very least, a service in their offerings that allows you to land logs—more specially, that is (focusing on the big three hyperscalers):
*   Amazon `CloudWatch Logs` ([http://mng.bz/yQjp](http://mng.bz/yQjp)) organizes different kinds of logs in log groups that, in turn, comprise one or more log streams (think of a never-ending series of events), which are made up of log events. It uses a proprietary way to ingest the logs and offers the embedded metric format (`EMF`; [http://mng.bz/nWa5](http://mng.bz/nWa5)), a format that allows you to embed custom metrics alongside logs. In other words, `EMF` is more event-centric (supporting high cardinality) than a pure log format. You can use `CloudWatch Logs Insights`, which provides a purpose-built query language to query your logs.
*   Azure `Azure Monitor Logs` ([http://mng.bz/MBGQ](http://mng.bz/MBGQ)) allows you to collect, organize, and analyze log and performance data from monitored resources in Azure (from resource groups to the app level). It organizes logs in workspaces, with queries written in Kusto Query Language (`KQL`), which is somewhat similar to what you find in `CloudWatch Logs Insights`. It sports an interesting feature called `functions`, enabling you to reuse query logic as well as rich export functionality (rule-based with various destinations, from storage to event hub).
*   Google Cloud Logging [https://cloud.google.com/logging/](https://cloud.google.com/logging/)) is part of the Google Cloud’s operations suite, and it includes storage, query, and log routing. As the top-level unit, Google Cloud Logging uses the log to organize entries. The log comprises log entries (events) that can stem from Google Cloud services or third parties (platform-level) as well as applications. What stands out in this context is how flexible the destinations for log routing are: from `JSON` files stored in `Cloud Storage` buckets (think the equivalent of `S3` buckets) to `BigQuery` (a large-scale, ad hoc `SQL` distributed query engine for data warehousing) to `Pub/Sub` (Google’s event stream) to things that are more comparable with what other clouds provide—that is, log “buckets” that support customizable retention periods.

Let’s have a look at a concrete logs example using CloudWatch. Examine the following CloudWatch Logs Insights query (where CloudWatch reports it scanned 3.7 GB of data, covering 3.1 million records):
```
fields @timestamp, @message     ❶
| sort @timestamp desc          ❷
| limit 25                      ❸
```
❶ Selects `fields` `@timestamp` and `@message` for output; we expect to see a table with two columns as a result.
❷ Sorts by time, descending, or more simply put, most recent event first
❸ Only shows 25 results in the output

Warning With the `limit` `25` in the preceding `CloudWatch Logs Insights` query example, we instructed CloudWatch to limit the output displayed to 25, but CloudWatch still needs to scan a large number of logs, depending on the time range selected. The time range, and with it the volume of the data scanned, is something you should always consider. On the one hand, it can take a long time, up to several minutes, for the query to return, which is far from interactive usage; and on the other hand, you’re oftentimes charged for the amount of data that has been scanned.

Figure 5.1 shows the result of the preceding `CloudWatch Logs Insights` query. Typically, cloud providers land all the system-level logs (sometimes called *platform logs*, such as `syslog` and `journald` but also cloud-platform/`API` logs) in their own offerings, which makes it a good default starting point, especially for all nonapplication-level logs. If you’re curious how to route your application-level logs to these destinations, refer to chapter 4, where we discussed options and usage for agents. Next up, let’s have a look at open source offerings in this space.
