
![](OEBPS/Images/05-01.png)

Figure 5.1 CloudWatch Logs Insights query result showing the 25 most recently added log events

### 5.2.2 Open source log backends

There are not that many widely used, open source–based log backends available (compared to metrics and traces, although as you will see later, log and trace backends share a lot of characteristics, and backends often store them similarly):
*   *Elasticsearch and OpenSearch*—I’m lumping Elasticsearch ([https://www.elastic.co/elasticsearch/](https://www.elastic.co/elasticsearch/)) and OpenSearch ([https://opensearch.org/](https://opensearch.org/)) together—ES/OS for short, going forward—because it really is a family of two projects and products (managed services) with a common root. They are both extending Apache Lucene ([https://lucene.apache.org/](https://lucene.apache.org/)), a veteran, comprehensive text search engine written in Java to make it easy to use for signal ingestion and query, even in a distributed setup. OpenSearch started out as a fork of Elasticsearch, sponsored by Amazon, and reached general availability ([http://mng.bz/a12j](http://mng.bz/a12j)) with its 1.0 version in July 2021. As time goes by, the ES/OS code bases are expected to diverge, but from the get-go, the major difference was the license under which each project was distributed. However, ES and OS have the fact that they are logs-first backends in common (optimized for storage and retrieval of logs). As a reminder, in chapter 4, we used OpenSearch as a destination for the Fluent Bit agent.
I won’t cover ES/OS in detail in this book, as there are plenty of resources available on the topic, so if you want to dive deep, I’d recommend reading the excellent second edition of the Manning book *Elasticsearch in Action* by Madhusudhan Konda (2023; [https://www.manning.com/books/elasticsearch-in-action-second-edition](https://www.manning.com/books/elasticsearch-in-action-second-edition)).
We will get back to the ES/OS topic in the context of traces later in this chapter, since this is a signal type ES/OS have decided to cover nowadays as well (support for metrics is also available; however, it is not as full blown as for the other two types).
*   *Grafana Loki (*[https://grafana.com/oss/loki/](https://grafana.com/oss/loki/)*)*—This is a horizontally scalable, multitenant log aggregation system, released under the AGPLv3 license, and its design was inspired by Prometheus. It does not index the contents of the logs themselves (as ES/OS do) but rather a set of labels for the ingested log streams. You have seen Loki in action already in chapter 2, and it’s certainly something to take a closer look at; currently, few managed offerings based on it exist.
*   *ZincObserve (*[https://github.com/zinclabs/zincobserve](https://github.com/zinclabs/zincobserve)*)*—This is a search engine you can use for indexing and looking up logs, with a focus on reducing log storage costs.
Now, let’s move on to commercial offerings in the space.

### 5.2.3 Commercial offerings for log backends

The commercial offering space for log backends is well set up, and you will find some common features across of all of these, such as paid ingestion, strong multitenancy support, and a wide range of built-in integrations with other signal sources, from apps to cloud provider offerings. The offerings include but are not limited to the following:
*   *Splunk (*[https://www.splunk.com/](https://www.splunk.com/)*)*—The established logs incumbent, with a range of enterprise features and strong OpenTelemetry commitment
*   *Instana (*[https://www.instana.com/](https://www.instana.com/)*)*—Acquired by IBM and provides many interesting features and integrations
*   *SolarWinds (*[https://www.solarwinds.com/](https://www.solarwinds.com/)*)*—Acquired formerly independent SaaS offerings Loggly and Papertrail as well as the monitoring offering Pingdom
*   *Logz.io (*[https://logz.io/](https://logz.io/)*)*—A start-up with an amazing range of offerings in the cloud-native domain (using OpenSearch and Jaeger)
No matter where you look, logs are always in use, so the question is not *if* but *which* backend you should use. We will get back to the selection criteria later in the chapter, but for now, you can rest assured that you will need a log backend. With this, you have a rough idea what logs backends can do and what options exist so let’s move on to metrics.

## 5.3 Backend destinations for metrics

Time series databases (TSDB) are the bread and butter of storing and querying signals of the metrics type. You can use TSDBs to track a range of different metrics, from low-level systems metrics, such as the memory usage of a container, to application-level metrics, such as the number of invocations of your service.
What is a TSDB, and how does it work? As shown in figure 5.2, the main index is the time. The unit of bookkeeping is the metric that comprises one or more time series. Each time series, in turn, keeps track of a value and, typically, you can have many of these time series active at the same time (in the sense that you can use them in a query, with their values readily being available). The typical way to interact with the TSDB is to ask, “In the time frame from `x` to `y`, what are the values or value aggregates matching these conditions?”
![](OEBPS/Images/05-02.png)

Figure 5.2 Time series database concept, showing `N` time series of the `mysvc_http_request_total` metric

The main index used in a TSDB is the time, usually divided into equal buckets. To keep things simple, figure 5.2 uses minute intervals, but usually, the intervals can be as small as seconds or milliseconds in granularity.
Each time series has a unique name (e.g., `mysvc_http_request_total{status_` `code="400"}`), capturing several dimensions of interest. The dimension of interest here is the HTTP status code returned (`status_code`), and many more, like the HTTP URL path, HTTP method, and so on might also be something you want to track. The metric this time series belongs to is of the type `counter`. That means its values increase strictly monotonically, and it captures the overall number of invocations of the `mysvc` service’s HTTP API. In this context, the net change between time periods will give you the number of invocations in a period. A single data point, addressed by time and unique time series, represents the value. In our case, as shown in figure 5.2, it means that at time `2022-08-01T15:04`, the TSDB has seen a total of 31 invocations that resulted in a `400` HTTP status code.
Oftentimes, you want to query a time range, rather than a particular instant in time. You can do this by specifying the start (`2022-08-01T14:58`) and end (`2022-08-01T15:01`). You can, for example, ask the TSDB how many invocations of a certain type occurred; in our case, if you’re interested in client errors, you’d ask how many `400` HTTP status codes were witnessed in the time frame, and the answer would be 12 (at the end of range, we have 24, and at the start, we had 12).

### Cardinality explosion

You might wonder what happens if you start adding dimensions to a metric you want to track, with a metric being the umbrella term for all related time series. In this example, let’s say a dimension you’re interested in is the user ID.
In contrast to, say, the HTTP status code, which has a small, bounded number of values (a couple of dozens, from 1xx to 5xx), the user ID could have many values. If your app is successful, potentially serving many hundreds of thousands of users, this means the TSDB has to ingest and store hundreds of thousands of time series (for that single metric only, and you may have thousands of metrics you want to track).
