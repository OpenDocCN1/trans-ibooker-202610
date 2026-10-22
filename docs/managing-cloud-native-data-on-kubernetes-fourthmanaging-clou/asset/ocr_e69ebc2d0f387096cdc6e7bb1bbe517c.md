# Stable identity for pods

First, StatefulSets provide a stable name and network identity for pods. Each pod is assigned a name based on the name of the StatefulSet, plus an ordinal number. For example, a StatefulSet called `cassandra` would have pods named `cassandra-1`, `cassandra-2`, `cassandra-3`, and so on, as shown in Figure 3-5. These are stable names, so if a pod is lost for some reason and needs replacing, the replacement will have the same name, even if it is started on a different worker node. Each pod’s name is set as it’s hostname, so if you create a headless service, you can actually address individual pods as needed, for example: `cassandra-1.cqlservice.default.svc.cluster.local`. We’ll discuss more about running Kubernetes Services for Cassandra later in this chapter in *Accessing Cassandra*.

![](bbox=[0.195, 0.47, 0.85, 0.775])

Figure 3-5. Sample Deployment of Cassandra on Kubernetes with StatefulSets

# Ordered lifecycle management