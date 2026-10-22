# Custom Resources

As we’ve discussed above, controllers can operate on built-in Kubernetes resources as well as [custom resources](custom resources). We’ve briefly mentioned this concept above, but let’s take this opportunity to define what custom resources are and how they extend the Kubernetes API.

Fundamentally, a custom resource is a piece of configuration data that Kubernetes recognizes as part of its API. While a custom resource is similar to a ConfigMap, it has a structure similar to built-in resources: metadata, specification, and status. The specific attributes of a particular custom resource type are defined in a *Custom Resource Definition*, or CRD. A CRD is itself a Kubernetes resource that is used to describe a custom resource.

In this book, we’ve been discussing how Kubernetes enables you to move beyond managing virtual machines and containers to managing virtual data centers. Custom resources provide the flexibility that helps make this a practical reality. Instead of being limited to the resources that Kubernetes provides off the shelf, you can create additional abstractions to extend Kubernetes for your own purposes. This is a critical component in a fast-moving ecosystem.

Let’s see what you can learn about custom resources from the command line. Use the kubectl api-resources command to get a listing of all of the resources defined in your cluster:

```
$ kubectl api-resources
NAME                 SHORTNAMES   APIVERSION   NAMESPACED   KIND
bindings                          v1           true         Binding
componentstatuses   cs            v1           false
ComponentStatus
configmaps          cm            v1           true         ConfigMap
...
```

As you look through the output, you’ll see many resource types introduced in previous chapters, along with their short names: StorageClass (sc), PersistentVolumes (pv), Pods (po), StatefulSets (sts), and so on. The API versions provide some clues as to the origins of each resource type. For example, resources with version v1 are core Kubernetes resources. Other