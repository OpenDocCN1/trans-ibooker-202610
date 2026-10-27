"inventory", and "village-linen-ac15e6". The last namespace matches the project ID used to deploy resources for use with these clusters (also the location where the cloud GKE cluster is deployed). The two application namespaces share a service account definition, though this will create two separate service accounts, one in each namespace. In the central namespace, we declare a ConfigMap which tells the applications which database to use, either the Cloud SQL or the on-premise cluster.

In each of the "child" namespaces ("website", "inventory", and "village-linen-ac15e6"), the repository has a HierarchyConfiguration object that enables the Hierarchy Controller to propagate objects from parent to child:

```
apiVersion: hnc.x-k8s.io/v1alpha1
kind: HierarchyConfiguration
metadata:
  name: hierarchy
spec:
  parent: central
```

In this case, all the "child" namespaces inherit from a common parent, but it is possible to "stack" these namespaces into a chain. For example, we could introduce another namespace - ‘applications’ - which inherits from ‘central’ and modify ‘inventory’ and ‘website’ to inherit from ‘applications’ instead. Performing this type of stacked hierarchy allows for a finer control of what is replicated, as well as adding additional tagging to the logs, if enabled.

When enabling Hierarchy Controller for a given cluster, an additional option can be selected which includes the tree labels on pods. These labels indicate the hierarchy relationship for the pod, and can be used both with command line tools, and in Cloud Logging, to filter for logs that descend from a given namespace.

Since Village Linen is using a hierarchy repo, explicitly defining the metadata.namespace field is not required for objects in the namespaces directory. However, the namespace itself is required to be explicitly defined; Village Linen has chosen to place these definitions in the namespace.yaml files, though that is not required. The objects defined in the website and inventory namespaces are used to enable the Multiple Repository functionality covered below. However, the Cloud SQL cluster is defined in