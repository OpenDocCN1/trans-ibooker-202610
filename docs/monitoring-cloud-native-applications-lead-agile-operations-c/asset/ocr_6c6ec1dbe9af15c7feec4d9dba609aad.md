| Kubernetes Metrics | Description |
|---|---|
| Pod | CPU usage rate by pod | CPU utilization % of a pod |
|  | Memory usage by pod | Memory usage by a pod |
|  | Bytes received rate by pod | Rate of bytes received over the network by the node |
|  | Bytes transferred rate by pod | Rate of bytes transferred over the network by the node |
|  | File system usage by pod | File system usage % by node |

# Operations in a Kubernetes Environment

Since Kubernetes doesn’t optimize performance directly by itself, developers have to identify and tune individual resources across the stack like hosts, clusters, containers, and applications. Every container and pod is different; some might be CPU hungry, whereas others might be running I/O-intensive workloads. To understand performance parameters in a Kubernetes environment, operators must ask these questions:

+   - What are the utilization levels across CPU and memory? Is there any node which is facing resource contention?
+   - What is the latency experienced by the pods? Do some pods face more latency than others in a cluster?
+   - Why is there a performance hit when autoscaling was initiated? Is autoscaling responsible for performance lag? Are there performance hits from multi-tenancy?
+   - Do underlying dependencies hamper performance of the applications?

It is the overall responsibility of both development and SRE teams to look into the performance of the application and ensure that overall SLOs are met:

+   - Developers define SLOs using metrics such as page load times, caching behavior, and transaction times for improving software quality.