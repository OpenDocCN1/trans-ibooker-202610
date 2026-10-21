```
completed" duration=7.944µs
level=info ts=2020-11-29T06:51:43.519Z caller=head.go:665
component=tsdb msg="Replaying WAL, this may take a while"
level=info ts=2020-11-29T06:51:43.520Z caller=head.go:717
component=tsdb msg="WAL segment loaded" segment=0 maxSegment=0
level=info ts=2020-11-29T06:51:43.520Z caller=head.go:722
component=tsdb msg="WAL replay completed"
checkpoint_replay_duration=93.857µs wal_replay_duration=695.77µs
total_replay_duration=812.641µs
level=info ts=2020-11-29T06:51:43.521Z caller=main.go:742
fs_type=19
level=info ts=2020-11-29T06:51:43.521Z caller=main.go:745
msg="TSDB started"
level=info ts=2020-11-29T06:51:43.521Z caller=main.go:871
msg="Loading configuration file" filename=prometheus.yml
level=info ts=2020-11-29T06:51:43.712Z caller=main.go:902
msg="Completed loading of configuration file"
filename=prometheus.yml totalDuration=190.710981ms
remote_storage=6.638µs web_handler=591ns query_engine=807ns
scrape=189.731297ms scrape_sd=42.312µs notify=322.934µs
notify_sd=20.022µs rules=3.538µs
level=info ts=2020-11-29T06:51:43.712Z caller=main.go:694
msg="Server is ready to receive web requests."
```

You can access the Prometheus UI on your browser at http://localhost:9090, and check out the various metrics that the Prometheus server is serving about itself at http://localhost:9090/metrics.

You can make use of the *Expression browser* now to basically run queries or write PromQL¹ expressions.

> **NOTE**
> 
> PromQL is Prometheus’s functional query language that lets you select and aggregate time series data in real time. The data can be visualized on the Prometheus UI in table or graph format.

To use the Expression browser, use any metric being exposed from localhost:9090/metrics, such as

```
prometheus_http_requests_total (which is a counter for all
```