PersistentVolumeClaim of the requested size of 1GB whenever it spins up a new pod within this StatefulSet.

# StatefulSet lifecycle management

Now that we’ve had a chance to discuss the components of a StatefulSet specification, you can go ahead and apply the source:

```
kubectl apply -f cassandra-statefulset.yaml
```

As this gets applied, you can execute the following to watch as the StatefulSet spins up Cassandra pods:

```
kubectl get pods -w
```

Let’s describe some of the behavior you can observe from the output of this command. First, you’ll see a single pod cassandra-0. Once that pod has progressed to Ready status, then you’ll see the cassandra-1 pod, followed by cassandra-2 after cassandra-1 is ready. This behavior is specified by the selection of podManagementPolicy for the StatefulSet. Let’s explore the available options and some of the other settings that help define how pods in a StatefulSet are managed.

The podManagementPolicy determines the timing of addition or removal of pods from a StatefulSet. The OrderedReady policy applied in our Cassandra example is the default. When this policy is in place and pods are added, whether on initial creation or scaling up, Kubernetes expands the StatefulSet one pod at a time. As each pod is added, Kubernetes waits until the pod reports a status of Ready before adding subsequent pods. If the pod specification contains a readinessProbe as we have done in this example, Kubernetes executes the provided command iteratively to determine when the pod is ready to receive traffic. When the probe completes successfully (i.e. with a zero return code), it moves on to creating the next pod. For Cassandra, readiness is typically measured by the availability of the CQL port (9042), which means the node is able to respond to CQL queries.