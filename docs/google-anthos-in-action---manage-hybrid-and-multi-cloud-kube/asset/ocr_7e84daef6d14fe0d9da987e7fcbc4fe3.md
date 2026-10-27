```
name: vili-cloud
labels:
  locality: cloud
---
kind: ClusterSelector
apiVersion: configmanagement.gke.io/v1
metadata:
  name: sel-clu-cloud
spec:
  selector:
    matchLabels:
      locality: cloud
```

In the cluster directory, we have configurations that apply to the cluster as a whole. These include ClusterRoles and ClusterRoleBindings, and Policy Controller constraints. By default, Hierarchy Controller only propagates RBAC Roles and RoleBindings from parent to child namespaces. However, Village Linen wants to use Hierarchy Controller to synchronize secrets and config maps from the ‘central’ namespace to the ‘application’ and ‘project’ namespaces. This way, secrets can be applied directly to the central namespace and replicated automatically without needing to be checked into a git repository. The modified HierarchyConfig is:

```
apiVersion: hnc.x-k8s.io/v1alpha1
kind: HNCConfiguration
metadata:
  name: config
spec:
  types:
    - apiVersion: v1
      kind: ConfigMap
      mode: propagate
    - apiVersion: v1
      kind: Secret
      mode: propagate
```

Moving up to the namespaces directory, we have a top-level file to define a set of RBAC roles to be created in each namespace. Village Linen can then bind these roles at either the ‘applications’ abstract namespace (which would apply the bindings to the ‘website’ and ‘inventory’ namespaces), or to the explicitly defined namespaces to control who has access to these roles.

The directory here defines a total of four namespaces: "central", "website",