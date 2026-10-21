HTTP requests to the Prometheus server). Then click the Graph tab and enter the metric in the Expression console, as shown in Figure 6-3.

![](bbox=[0.14, 0.15, 0.86, 0.43])

Figure 6-3. Expression browser result for prometheus_http_requests_total

The query language is very extensive and supports a number of functions and operators on the metrics. For example, if you want to count the number of returned time series for the metric prometheus_target_interval_length_seconds, you can issue the following command in the Expression browser:

```
count(prometheus_target_interval_length_seconds)
```

Similarly, if you want to find out how fast a metric is increasing per second, you can use rate in the Expression browser:

```
rate(prometheus_tsdb_head_samples_appended_total[1m])
```

# node_exporter

The node_exporter exposes an array of host-level metrics, such as CPU, memory, disk space, I/O, and network bandwidth, along with a variety of kernel-level metrics. You can download the node-exporter from