Server sends *watch events* to controllers to notify them of changes in state for resource types for which they have registered interest, such as the creation or deletion of a resource, or an event occurring on the resource.

For data infrastructure these changes could include a change in the number of requested replicas for a cluster, or a notification that a pod containing a database replica has died. Because there could be many such updates occurring in a large cluster, controllers frequently make use of caching.

# *Making changes to the state of resources*

This is the core business logic of a controller - comparing the state of resources to their desired state and executing actions to change the state to the desired state. In the Kubernetes API, the current state is captured in .status fields of resources, and the desired state is expressed in terms of the .spec field. The changes could include invocations of the Kubernetes API to modify other resources, administrative actions on the application being managed, or even interactions outside of the Kubernetes cluster.

For example, consider a controller managing a distributed database with multiple replicas. When the database controller receives a notification that the desired number of replicas has increased, the controller could scale an underlying Deployment or StatefulSet that it is using to manage replicas. Later, when receiving a notification that a pod has been created to host a new replica, the controller could initiate an action on one or more replicas in order to rebalance the workload across those replicas.

# *Updating the status of resources*

In the final step of the control loop, the controller updates the .status fields of the resource using the API server, which in turn updates that state in etcd. You’ve viewed the status of resources like Pods and Persistent Volumes in previous chapters using the kubectl get and kubectl describe commands. For example, the status of a Pod includes