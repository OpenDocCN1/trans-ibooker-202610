associated with changing the address of the data itself mid-flight. Applications typically bind addresses to physical locations at the beginning of the execution, and these bindings cannot be easily changed. Furthermore, depending on the size of the data that requires moving, bandwidth and read/write throughput constraints cannot be overlooked. As size increases, data acquires more gravity and loses mobility.

As a result, VMs that required access to data residing in persistent storage became bound to moving only to other physical computing resources where the VM could still have access to the storage where the data was initially stored. In essence, the computing workload moved but the data did not.

Containers added a level of flexibility to mobility when it made it viable and easier to move workloads by simply starting new containers in the newly desired destinations and then terminating the old ones, as opposed to actually moving a running workload. Nevertheless, similar constraints applied to containers that accessed persistent storage, referred to as stateful containers. As long as the new location of a stateful container had access to the same persistent storage that the old container was using in its prior location, mobility was easier. An interesting aspect emerges with containers however. Because the container is re-starting from scratch, new bindings between the container and the physical storage can be made. As a result, containers can also be moved to locations where replicas of the data exist, as long as the replicas are an exact mirror of each other.

# 15.1.3 Chapter Organization

This chapter provides a glimpse into the topic of Anthos, data and analytics, and it is organized as follows:

+   - Section Kubernetes and Storage reviews some important concepts on how Kubernetes interfaces with the storage layer, forming a foundation for the next sections;
+   - Section Anthos and Storage discusses how Anthos adds value on top of Kubernetes to enhance the portability of workloads that access permanent storage;
+   - Section BigQuery Omni Powered by Anthos explains how Anthos has