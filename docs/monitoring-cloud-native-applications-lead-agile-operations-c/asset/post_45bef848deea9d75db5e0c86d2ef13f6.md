
## TSDB

Time-series data needs a purpose-built database which is designed to handle the ever-increasing massive amounts of timestamped metrics and events data. Most modern monitoring tools store monitoring data in a TSDB which is optimized to store timestamped data. Metrics and events that are timestamped can be easily stored, analyzed, and aggregated over time in this database, whether it be server metrics, network flow data, sensor data, application performance monitoring data, clickstream data, events, or just about any kind of data having a time index. Once the data is stored in the TSDB it is easy to measure its rate of change.

Key properties of a TSDB are

- **High write throughput:** In a monitoring system, data is collected very frequently and then written to a database. The data is written more often than it is read from, with typically 90%-95% being write operations. Therefore, the back-end database should be able to support a very high number of write operations. TSDBs are designed for high concurrent writes and provide a very high write throughput.
- **Data summarization:** In a monitoring system, there are requests made for summarized data over a particular time range which can range from a few days to a few months. This is done to establish patterns and identify failures which might have gone unnoticed in the past. The database query can actually involve an inordinate amount of time-series data and the database should be able to aggregate all this data without any noticeable latency. In the case of a TSDB, a user can write interactive queries on the data and get results instantaneously without having to wait for data processing and data summarization. This is very difficult to achieve optimally using relational databases.
- **Data rollup and aggregation:** In a monitoring system, generally high-precision data that is a few days old is kept as is, whereas data which has become older is aggregated with fewer samples from the data retained. This is termed as downsampling. The benefit of a TSDB is that it can do data aggregation and rollup on its own and no complex procedure is needed to be maintained by the database admins.
- **Data comparison:** In a TSDB, every new entry and every change are appended only and added as a new row to measure the change with respect to time. This is useful to calculate variance with previously observed values.
- **Massive storage:** Another design consideration for a TSDB is to store a massive amount of information, sometimes even ranging to petabytes (PB) and at the same time to be able to support high availability and a distributed architecture to be able to support multiple concurrent writes.

These are the some of the key design attributes of a TSDB. It can efficiently handle massive amounts of timestamped data while at the same time working on data management and summarization out of the box. By understanding the important characteristics of time-series data and the basic design principles behind a TSDB, we can clearly ascertain that NoSQL databases that use LSM-Tree-based storage engines have significant advantages over B-Tree-based relational database management systems.

LSM-Tree-based TSDBs, as we previously saw, are designed to optimize the writes, and in some cases their write performance reaches ten times higher when compared with B-Tree-based RDBMS. In contrast, their read performance is poor. LSM-Tree-based TSDBs are suited only for scenarios that are highly skewed towards writes rather than reads. Most of the well-known open source TSDBs use LSM-Tree-based storage engine; for instance, OpenTSDB uses HBase as the underlying storage engine, Prometheus relies on LSM-based LevelDB storage engine, and InfluxDB uses a Time Structured Merge (TSM) storage engine, which was developed on top of LSM. We will learn more about these storage engines when we discuss these specific open source tools in more detail in Part 2 of this book.

## Time-Series Data Models

A time-series data model consists of the following:

- **Timestamp:** The time-indexed value for each data point measured.
- **Subject:** It refers to the component being measured; for example, in the case of CPU usage, the subject would be the host. A subject can further have multiple attributes and dimensions.
- **Measurement:** Measurement is the metric along with its value which we are trying to measure, which in the case of CPU can be CPU usage percentage or in the case of storage disk can be disk IOPS.

### Modeling by Data Source

In this model, all metrics from a single data source at a given time are stored in the same row as fields. For example, in Figure 3-7, both CPU usage percentage and disk IOPS for host 1 are captured and entered as one single entry for the first timestamp at 14:00:00Z. This would help to write queries that join multiple fields under one single measurement.

| timestamp | datacenter | hostname | cpu | disk iops |
|---|---|---|---|---|
| 2020-08-26T 14:00:00Z | Datacenter 1 | host 1 | 30 | 500 |
| 2020-08-26T 14:00:00Z | Datacenter 1 | host 2 | 15 | 100 |
| 2020-08-26T 14:00:10Z | Datacenter 1 | host 1 | 33 | 750 |
| 2020-08-26T 14:00:10Z | Datacenter 1 | host 2 | 12 | 100 |
| 2020-08-26T 14:00:20Z | Datacenter 1 | host 1 | 35 | 800 |
| 2020-08-26T 14:00:20Z | Datacenter 1 | host 2 | 20 | 200 |
| 2020-08-26T 14:00:30Z | Datacenter 1 | host 1 | 32 | 650 |
| 2020-08-26T 14:00:30Z | Datacenter 1 | host 2 | 22 | 150 |

Figure 3-7. Modeling by data source

### Modeling by Metrics

In the model shown in Figure 3-8, metrics drive the data point, as each row represents a certain metric and its measurement at a given time. For example, both CPU usage percentage and disk IOPS have a separate row entry for a given timestamp for host 1.

| metric | timestamp | datacenter | hostname | metric value |
|---|---|---|---|---|
| cpu | 2020-08-26T 14:00:00Z | Datacenter 1 | host 1 | 30 |
| cpu | 2020-08-26T 14:00:00Z | Datacenter 1 | host 2 | 15 |
| cpu | 2020-08-26T 14:00:10Z | Datacenter 1 | host 1 | 33 |
| cpu | 2020-08-26T 14:00:10Z | Datacenter 1 | host 2 | 12 |
| cpu | 2020-08-26T 14:00:20Z | Datacenter 1 | host 1 | 35 |
| cpu | 2020-08-26T 14:00:20Z | Datacenter 1 | host 2 | 20 |
| cpu | 2020-08-26T 14:00:30Z | Datacenter 1 | host 1 | 32 |
| cpu | 2020-08-26T 14:00:30Z | Datacenter 1 | host 2 | 22 |
| disk iops | 2020-08-26T 14:00:00Z | Datacenter 1 | host 1 | 500 |
| disk iops | 2020-08-26T 14:00:00Z | Datacenter 1 | host 2 | 100 |
| disk iops | 2020-08-26T 14:00:10Z | Datacenter 1 | host 1 | 750 |
| disk iops | 2020-08-26T 14:00:10Z | Datacenter 1 | host 2 | 100 |
| disk iops | 2020-08-26T 14:00:20Z | Datacenter 1 | host 1 | 800 |
| disk iops | 2020-08-26T 14:00:20Z | Datacenter 1 | host 2 | 200 |
| disk iops | 2020-08-26T 14:00:30Z | Datacenter 1 | host 1 | 650 |
| disk iops | 2020-08-26T 14:00:30Z | Datacenter 1 | host 2 | 150 |

Figure 3-8. Modeling by metrics

## Data Analysis and Query Engine

Once the telemetry data has been collected and stored in the TSDB, the next step is to analyze the data. Common data analysis methods include filtering, aggregation, grouping, and downsampling. Data analysis is done through a query engine which can easily work on the stored data and provide the queried results. The query engines generally use queries that are modifications on top of SQL like query languages to query the data. In this section, we will look at the data analysis methods and also learn about the various mechanisms used to query telemetry data inside TSDBs.

### Filter

Filtering is the most common process in which we run a query on the complete dataset and it returns only those datapoints that match the conditions set out in the query. In the case of time-series analysis, the filtering starts from a metric value and then refines it for time range or measurements as per the query. In the following example for the sample time-series data, we first select the datapoints from the table with the metric ‘cpu’ and then perform a more detailed query based on other conditions such as ‘datacenter’ or ‘hostname’.

In order to explain the concept of filtering, we are using the sample data shown in Figure 3-9:

| metric | timestamp | datacenter | hostname | metric value |
|---|---|---|---|---|
| cpu | 2020-08-26T 14:00:00Z | Datacenter 1 | host 1 | 30 |
| cpu | 2020-08-26T 14:00:00Z | Datacenter 1 | host 2 | 15 |
| cpu | 2020-08-26T 14:00:10Z | Datacenter 1 | host 1 | 33 |
| cpu | 2020-08-26T 14:00:10Z | Datacenter 1 | host 2 | 12 |
| cpu | 2020-08-26T 14:00:20Z | Datacenter 1 | host 1 | 35 |
| cpu | 2020-08-26T 14:00:20Z | Datacenter 1 | host 2 | 20 |
| cpu | 2020-08-26T 14:00:30Z | Datacenter 1 | host 1 | 32 |
| cpu | 2020-08-26T 14:00:30Z | Datacenter 1 | host 2 | 22 |
| disk iops | 2020-08-26T 14:00:00Z | Datacenter 1 | host 1 | 500 |
| disk iops | 2020-08-26T 14:00:00Z | Datacenter 1 | host 2 | 100 |
| disk iops | 2020-08-26T 14:00:10Z | Datacenter 1 | host 1 | 750 |
| disk iops | 2020-08-26T 14:00:10Z | Datacenter 1 | host 2 | 100 |
| disk iops | 2020-08-26T 14:00:20Z | Datacenter 1 | host 1 | 800 |
| disk iops | 2020-08-26T 14:00:20Z | Datacenter 1 | host 2 | 200 |
| disk iops | 2020-08-26T 14:00:30Z | Datacenter 1 | host 1 | 650 |
| disk iops | 2020-08-26T 14:00:30Z | Datacenter 1 | host 2 | 150 |

Figure 3-9. Original sample data

The query selects cpu from table where datacenter= ‘Datacenter 1’ and hostname= ‘host 1’; when run on the preceding table, this will provide the table shown in Figure 3-10:

| metric | timestamp | datacenter | hostname | metric value |
|---|---|---|---|---|
| cpu | 2020-08-26T 14:00:00Z | Datacenter 1 | host 1 | 30 |
| cpu | 2020-08-26T 14:00:10Z | Datacenter 1 | host 1 | 33 |
| cpu | 2020-08-26T 14:00:20Z | Datacenter 1 | host 1 | 35 |
| cpu | 2020-08-26T 14:00:30Z | Datacenter 1 | host 1 | 32 |

Figure 3-10. Filtered data

The resulting table is filtered on the basis of datacenter and hostname to return results which satisfy the queried values equal to ‘Datacenter 1’ and ‘host 1.’ This is a simple example to show the mechanism of filtering telemetry data from a given dataset.
