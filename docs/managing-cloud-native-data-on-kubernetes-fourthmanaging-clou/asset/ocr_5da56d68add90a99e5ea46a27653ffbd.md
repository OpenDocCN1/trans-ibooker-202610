![](bbox=[0.15, 0.09, 0.86, 0.56])

Figure 6-8. Monitoring Cassandra with Kube-Prometheus Stack

The right side of Figure 6-8 shows how Prometheus and Grafana are configured to consume and expose the Cassandra metrics. The K8ssandra Operator creates ServiceMonitor resources for each CassandraDatacenter for which monitoring has been enabled. The ServiceMonitor is a CRD defined by Prometheus Operator which contains configuration details describing how to collect metrics from a set of Pods, including the following:

+   - A `selector` that references the name of a label which identifies the Pods
+   - Connection information such as the `scheme` (protocol), `port`, and `path` to use to gather metrics from each Pod