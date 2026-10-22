

Kubernetes SREs love the idea of elastic workloads, but in Spark, that’s been much more painful than it needs to be. Regular execution of a Spark job can cause a rapid increase in Pods while the job is processing, and then those resources are released when the job is complete. Scaling up is, unfortunately, much easier than scaling down. Spark can now make use of externalized resource allocation that opens the possibility for better solutions. Open source projects such as Apache YuniKorn and Volcano are already working on resource allocation solutions inside of Kubernetes, which the execution allocator and shuffle service can leverage to align more closely to the way Kubernetes works. Taken together, these efforts provide a significant path forward for more efficient resource usage in a dynamic Spark environment.

Resource-intensive applications like Spark can benefit from having more insight from the underlying systems to balance jobs as they are run. Today, Kubernetes and Spark do not share as much information as they need for the best outcomes, and you should adjust your expectations accordingly. A specific example is the way Kubernetes approaches storage quota enforcement, which can work against Spark in some cases. Ephemeral storage is a key part of Spark execution, but Kubernetes provides no system-level APIs that Spark could use to check the utilization against the storage quota, which can cause job failures. Previous applications that have been deployed on Kubernetes haven’t needed this level of insight. Spark creates a compelling case for significant changes to expose additional system level APIs in Kubernetes, which will make it possible to build much more reliable analytic workloads.

If you are interested in solving these problems, these solutions will happen in open source projects. You can get involved today and help us move forward. Apache Spark has room for improvement and so does Kubernetes, but the future direction is clear. Kubernetes is where cloud native analytics will happen, and Spark will continue to evolve. There will be a lot of interesting work in this area for the next several years at least, and it’s a pretty exciting community to be a part of.

Engineers can be their own worst enemies. Often when we go to solve one problem, it creates a few more that need to be solved. We can count this as progress, however, when it comes to managing data. Every step up we take, despite the challenges, allows for new solutions that were never available before. It’s staggering to think of how much is possible with just a small number of people where not too long ago, it took massive teams to accomplish anything close to the analytics required today. See the quote about laziness at the beginning of the chapter. There is still work to be done and next, we will look at what tools are available for analyzing data in Kubernetes.

## Introduction to Apache Spark™

Google changed the world of data analytics with the MapReduce algorithm simply by describing it to the world in an academic paper. Not long after the MapReduce paper got engineers talking, an open source implementation was created, the now-famous Apache Hadoop™. A massive ecosystem was built up around Hadoop with tooling and complementary projects such as Hadoop Distributed File System (HDFS). Growing pains from this fast-moving project opened the door for the next generation of tools that built on the lessons learned with Hadoop. One project that grew in popularity as an alternative to Hadoop was Apache Spark™. Spark addressed reliability and processing efficiency problems by introducing the Resilient Distributed Dataset (RDD) API and Directed Acyclic Graphs (DAG).

The RDD was a significant improvement over the forced linear processing patterns of MapReduce. This involved a lot of reading from disk, processing, and then writing back to disk only to be redone over and over. This put a burden on developers to reason through how data was processed. RDDs shifted the responsibility away from developers as an API that created a unified view of all data while abstracting the actual processing details. Those processing details were created in a workflow to perform each task expressed in a DAG. The DAG is nothing more than an optimized path that describes data and operations to be completed in an orderly
fashion until the final result is produced. RDDs were eventually replaced with the DataSet and DataFrame APIs, which further enhanced developer productivity over large volumes of data.

Spark’s operational complexity is greatly reduced compared to Hadoop, which notoriously tipped the scale with the infrastructure required even for basic jobs. Spark is an excellent example of one of the benefits of being a next-generation implementation with great hindsight. Much effort was put into simplifying Spark’s architecture, leveraging distributed systems concepts. The result is the three familiar components you should be familiar with in a Spark Cluster shown in Figure 9-2.

![](img/cb55ccac39ceccbc2806f8db5c8fad69_273_0.png)

Figure 8-2. Components of a Spark Cluster

Let’s review the responsibilities of each of these components:

### Cluster Manager

The Cluster Manager is the central hub for activity in the Spark cluster where new jobs are submitted for processing. The Cluster Manager also acquires the resources needed to complete the task submitted. Different
versions of the Cluster Manager are primarily based on how resources are managed. (Standalone, YARN, Mesos, and Kubernetes) The Cluster Manager is critical for deploying your Spark application using Kubernetes.

### Worker Node

When Spark jobs run, they are broken into manageable pieces by the Cluster Manager and handed to the Worker Nodes to perform the processing. They serve as the local manager for hardware resources as a single point of contact. Worker Nodes invoke and manage Spark Executors.

### Spark Executor

Each application sent to a Worker Node will get its own Spark Executor. Each Executor is a stand-alone JVM process that operates independently and communicates back with the Worker Node. The tasks for the application are broken into threads that consume the compute resources allocated.

## Deploying Apache Spark™ in Kubernetes

As of Apache Spark version 2.3, Kubernetes is one of the supported modes in the Cluster Manager. It would be easy to understate what that has meant for Spark as a cloud native analytics tool. Starting with Spark 3.1, Kubernetes mode is considered production-ready, continually adding steady improvements. When the Spark project looked at what it takes to run a clustered analytics system inside a cluster orchestration platform, there were a lot of overlaps that became obvious. Kubernetes already had the mechanisms in place for the lifecycle management of containers and the dynamic provisioning and de-provisioning of compute elements, so Spark lets Kubernetes take care of this work. The redundant parts were removed, and Spark is closer to how Kubernetes works as a result. The spark-submit command-line tool was extended to interface with Kubernetes
clusters using the Kubernetes API, maintaining a familiar toolchain for developers and data engineers. These unique aspects of a Spark deployment in Kubernetes are shown in Figure 9-3.

![](img/cb55ccac39ceccbc2806f8db5c8fad69_275_0.png)

Figure 8-3. Spark on Kubernetes

Let’s highlight a few of the differences:

### Spark Driver

The dedicated Cluster Manager of a standalone Spark cluster is replaced with native Kubernetes cluster management and the Spark Driver for Spark-specific management. The Spark Driver Pod is created when the Kubernetes API server receives a job from the spark-submit tool. It invokes the Spark Executor Pods to satisfy the job requirements. It is also responsible for cleaning up Executor Pods after the job, making it a crucial part of elastic workloads.

### Spark Executor

Like a stand-alone Spark cluster, Executors are where the work gets done and where the most compute resources are consumed. Invoked from the Spark Driver, they take job instructions passed by spark-submit with details such as CPU and memory limits, storage information, and security credentials. The containers used in Executor Pods are pre-created by the user.

### Custom Executor Container

Before a job is sent for processing using `spark-submit`, users must build a custom container image tailored to meet the application requirements. The Spark distribution download contains a Dockerfile that can be customized and used in conjunction with the `docker-image-tool.sh` script to build and upload the container required when submitting a Spark job in Kubernetes. The custom container has everything it needs to work within a Kubernetes environment like a Spark Executor based on the Spark distribution version required.

The workflow for preparing and running Spark jobs when using Kubernetes and defaults can be relatively simple only requiring a couple of steps. This is especially true if you are already familiar with and running Spark in production. You will need a running Kubernetes cluster and a download of Apache Spark in a local file path along with your Spark application source code.

### Build your custom container

An executor container encapsulates your application and the runtime needed to act as an Executor Pod. The build script takes an argument for the source code repository and a tag assignment for the output image when pushed to your Docker registry.

```
$ ./bin/d
```
