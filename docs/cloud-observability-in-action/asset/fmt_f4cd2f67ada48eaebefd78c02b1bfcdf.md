
This idea of many different values a dimension can take on being multiplied by the number of metrics is what we call cardinality explosion and can result in slow query times, since the `TSDB` needs to inspect each time series in a given time range, for each dimension. That systems property makes `TSDBs` for certain types of observability use cases not the best fit: at the very least, you should be aware of this and avoid adding dimensions that you know or suspect of “exploding,” in terms of their cardinality.

If you would like to dive deeper into the topic of `TSDBs` and their use cases in general, consider reading the article “What Is Time-Series Data? Definitions & Examples” by Ajay Kulkarni, Ryan Booz, and Attila Toth ([https://www.timescale.com/blog/time-series-data/](https://www.timescale.com/blog/time-series-data/)). Now, let’s switch gears and get practical by looking at several open source and commercial offerings you can use as `TSDB` backends, especially in the context of `Prometheus`-compatible cases. We covered the `Prometheus` basics in chapter 4 and will not repeat them here. We will focus on mid- to large-scale production use cases in this chapter—in other words, when a single `Prometheus` instance typically is not sufficient anymore. Let’s again look at the options for cloud providers, open source offerings, and commercial offerings.

Note `Prometheus` is, by design, not horizontally scalable. That is, you can add more RAM and locally attached persistent storage to scale `Prometheus` vertically to handle more metrics or deal with a longer time period. What you can’t expect is that running multiple copies of `Prometheus` automatically means the telemetry data is managed for you in such a distributed setup.

If you do have use cases such as a fleet of `Kubernetes` clusters, where you want to land metrics from many machines in a single place (federation), or you want to be able to query long time periods, such as the past three years, and a single `Prometheus` instance can’t handle the task, you are looking for long-term storage (LTS) solutions. The following options can handle these use cases.

### 5.3.1 Cloud providers

Every cloud provider has, at the very least, a service in their offerings that allows you to ingest metrics as well as some open source or `Prometheus`-based ones, in addition to their native offerings. The following are some of the most prominent hyperscaler options, offering both platform and application-metrics ingestions:

* `Amazon CloudWatch Metrics` ([http://mng.bz/gBne](http://mng.bz/gBne)) — Covers metrics from over 100 AWS services (vended metrics) and keeps them around for 15 months, allowing you to query up-to-the-minute data and historical data equally. You can use the property query language as well as `SQL` (via CloudWatch Metrics Insights) to query metrics as well as alert (called alarms) on certain conditions. It also comes with dashboards that provide insights out of the box.

* `Amazon Managed Service for Prometheus` ([http://mng.bz/e169](http://mng.bz/e169)) — A `Prometheus`-compatible monitoring service that uses `CNCF Cortex` in its data plane. You can ingest metrics via the `Prometheus` `remote_write` interface ([http://mng.bz/pPe5](http://mng.bz/pPe5)) and use `PromQL` to query it.

* `Amazon Timestream` ([http://mng.bz/Ox0K](http://mng.bz/Ox0K)) — A serverless time series database service for IoT and operational applications that offers an `SQL`-based query language and enables you to use it in a more traditional big data use case.

* `Azure Monitor Metrics` ([http://mng.bz/Y1Bo](http://mng.bz/Y1Bo)) — Allows you to collect, organize, and analyze metrics from monitored resources in Azure. It supports namespaces to logically group time series as well as rich exploration and fine-grained storage and retention options.

* `Google Cloud Monitoring` ([https://cloud.google.com/monitoring](https://cloud.google.com/monitoring)) — Part of the Google Cloud’s operations suite; it enables you to collect metrics, events, and metadata from Google Cloud, AWS, hosted uptime probes, and application instrumentation.

* `Google Cloud Managed Service for Prometheus` ([http://mng.bz/GyaM](http://mng.bz/GyaM)) — Runs `Prometheus`-based collectors as `DaemonSets` in `Kubernetes` and ensures scalability by only scraping targets on colocated nodes and pushing the scraped metrics to the central data store (`Monarch`). You can use `PromQL` to query the metrics, for alerts or, for example, as a data source in `Grafana`.

Figure 5.3 shows an example metrics explorer output—here, the Metrics Explorer being part of the `Azure Monitor Metrics` service. Next, let’s look at open source offerings for metrics LTS and federation.

![](OEBPS/Images/05-03.png)

Figure 5.3 Azure Metrics Explorer example output (Azure docs, [http://mng.bz/Y1Bo](http://mng.bz/Y1Bo))

### 5.3.2 Open source metrics backends

The metrics backend space is rather crowded, which is good for you, in terms of options available, but it can also be challenging to keep track of developments and make the best decision for workload:

* `CNCF Cortex and Thanos` — Two CNCF projects with different philosophies about how to ingest and query `Prometheus` timeseries. `Cortex` ([https://cortexmetrics.io/](https://cortexmetrics.io/)) is used in `Amazon Managed Service for Prometheus`; and `Thanos` ([https://thanos.io/](https://thanos.io/)), founded by Improbable, is used in a variety of cases by many large companies, from Amadeus to eBay to SoundCloud to Tencent. `Cortex` has a push approach (using the `remote_write` `Prometheus` interface) to allow a `Prometheus` fleet to ingest metrics, whereas `Thanos` initially had a pull approach (using the `remote_read` API), allowing one to federate metrics from many `Prometheus` setups (but now, it also supports push).

* `Clymene` ([https://clymene-project.github.io/](https://clymene-project.github.io/)) — A time series data and logs collection platform for distributed systems that allows the storage of signals in different types of databases, such as for logs or metrics. It is a relatively new system, but in its design, it is cloud native.

* `Grafana Mimir` ([https://grafana.com/oss/mimir/](https://grafana.com/oss/mimir/)) — A horizontally scalable, highly available, multitenant, long-term storage option for `Prometheus`, available under the `AGPLv3` license. It started in 2022 as a fork of the `CNCF Cortex` project, and Grafana Labs has added many improvements, making it part of their SaaS offering as well.

* `Icinga` ([https://github.com/icinga/icinga2](https://github.com/icinga/icinga2)) — An open source monitoring system that checks the availability of your network resources, notifies users of outages, and generates performance data for reporting. You don’t find `Icinga` widely used in cloud-native setups, but you might still need to be able to interop with it.

* `InfluxDB` ([https://github.com/influxdata/influxdb](https://github.com/influxdata/influxdb)) — A popular open source time series platform spanning a wide range of functionality, including offline process of the data, alerting, and exploration, with good `Prometheus` compatibility.

* `LinDB` ([https://github.com/lindb/lindb](https://github.com/lindb/lindb)) — A scalable, high-performance, and highly available distributed time series database that features its own `SQL`-like query language, called `LinQL`.

* `M3DB` ([https://m3db.io/](https://m3db.io/)) — Originally from Uber, this time series data store is used by Walmart, LinkedIn, and many others. It’s capable of handling billions of metrics (see this FOSDEM 2020 talk on its scaling: [https://archive.fosdem.org/2020/schedule/event/m3db/](https://archive.fosdem.org/2020/schedule/event/m3db/)) is now also available via Chronosphere as a managed service.

* `Nagios Core` ([https://github.com/NagiosEnterprises/nagioscore](https://github.com/NagiosEnterprises/nagioscore)) — An open source app that enables you to monitors systems, networks, and infrastructure.
