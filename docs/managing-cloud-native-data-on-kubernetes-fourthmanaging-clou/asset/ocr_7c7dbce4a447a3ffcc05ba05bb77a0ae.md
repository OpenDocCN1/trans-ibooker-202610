Executor Pods that the Spark Operator will deploy. For `driver`: only one core is required but cpu and memory allocations need to be enough to maintain the number of executors you require. The number is set in the `executor:` section under `instances:`.

# MINDING YOUR RESOURCES

For resource management, the requests you make under `driver:` and `spec:` need to be carefully considered for resource management. The number of instances plus their allocated CPU and memory could use up resources quickly. Jobs can hang indefinitely while waiting for resources to free up, which may never happen.

Now that your configuration yaml is ready, it’s time to put it into action. For a walk-through, refer to Figure 9-4.

![](bbox=[0.16, 0.44, 0.83, 0.83])

Figure 8-4. Spark on Kubernetes Operator