make_pod_spec method or specifying a YAML configuration file. It will connect to the default Kubernetes cluster accessible via kubectl and invoke the cluster creation inside your Kubernetes cluster as a part of the running Python program.

```
from dask.distributed import Client
from dask_kubernetes import KubeCluster, make_pod_spec

pod_spec = make_pod_spec(image='daskdev/dask:latest',
                        memory_limit='4G', memory_request='4G',
                        cpu_limit=1, cpu_request=1)

cluster = KubeCluster(pod_spec)

# specify the number of workers(pods) explicitly
cluster.scale(10)

# or dynamically scale based on current workload
cluster.adapt(minimum=1, maximum=100)

# Connect Dask to the cluster
client = Client(cluster)

# Your Python code here
```

Developer access to Kubernetes clusters for parallel computing couldn’t get much easier and this is the appeal new tools like Dask can provide.

# Ray

In a significant difference from the arbitrary Python code in Dask, Ray takes a different approach to Python clustering by operating as a parallel task manager that includes a distributed computing framework. For the end-user, Ray provides low-level C++ libraries to run distributed code purpose-built for compute-intensive workloads typical in data science. The base is Ray Core which does all the work of distributing workloads using the concept of a task. When developers write Python code using Ray each task is expressed as a remote function, as shown in this example from the Ray. documentation:

```
# By adding the `@ray.remote` decorator, a regular Python function
# becomes a Ray remote function.
```