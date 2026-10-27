

Label selectors allow you to filter Kubernetes objects based on labels. You can combine multiple requirements and use the following set-based operators to build complex selectors.

| Operator | Description |
|---|---|
| In | Checks the label against a list; if any value is in the list, it is considered to be a match. |
| NotIn | Checks the label against a list; if the value is not in the list, it is considered to be a match. |
| Exists | Checks if the label exists; if it does, it is considered to be a match.<br>Note: The value of the label does not matter and is not evaluated in the match. |
| DoesNotExist | Checks if the label does not exist; if the label does not match any in the list, it is considered to be a match.<br>Note: The value of the label does not matter and is not evaluated in the match. |
| Gt | Used to compare numeric values in a label; if a value is greater than (Gt) the label, it is considered to be a match.<br>Note: Only works with a single number. |
| Lt | Used to compare numeric values in a label; if a value is less than (Lt) the label, it is considered to be a match.<br>Note: Only works with a single number. |

Affinity rules are used to constrain which nodes a Pod can be scheduled on. There are two types of node affinity:

- `RequiredDuringSchedulingIgnoredDuringExecution` — known as a hard affinity requirement. The rule must be met for the Pod to be scheduled.
- `PreferredDuringSchedulingIgnoredDuringExecution` — known as a soft affinity requirement. The scheduler tries to satisfy the rule, but if no matching node is available, the Pod can still be scheduled.

## Creating node affinity rules

Node affinity is defined in a PodSpec under the `affinity` field. To create a node affinity rule, you add the node affinity field to the Pod specification and use node labels to determine which nodes are eligible.

| Node | Node Labels |
|---|---|
|  | beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=e2-medium,beta.kubernetes.io/os=linux,cloud.google.com/gke-nodepool=default-pool,cloud.google.com/gke-os- |

*The `topologyKey` is used by Kubernetes to create a list of nodes that will be checked against the affinity rule. Using a `topologyKey`, you can decide to look for matches based on different filters like zones, or nodes.*

### Creating pod affinity rules

Pod affinity is used to schedule Pods onto nodes that already have certain Pods running. For example, to schedule a Pod on the same node as an existing Pod with the label `license=widgets`, you can use the following manifest:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: widgets-license-example
spec:
  affinity:
    podAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchExpressions:
          - key: license
            operator: In
            values:
            - widgets
        topologyKey: kubernetes.io/hostname
  containers:
  - name: nginx-widgets
    image: nginx-widgets
```

This manifest instructs Kubernetes to place the `nginx-widgets` Pod on a node that already runs a Pod with the label `license=widgets`.

## Creating pod anti-affinity rules

Pod anti-affinity is the opposite of pod affinity. While affinity rules are used to attract Pods to nodes, anti-affinity rules are used to spread Pods across different nodes or topology domains.

The following example creates a Pod with a preferred anti-affinity rule. It avoids scheduling the Pod in the same zone as Pods labeled `nginx-frontend`, but if no other zone is available, the scheduler can still place it elsewhere.

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: nginx-frontend-antiaffinity-example
spec:
    podAntiAffinity:
        preferredDuringSchedulingIgnoredDuringExecution:
            podAffinityTerm:
                labelSelector:
                    matchExpressions:
                    - key: app
                      operator: In
                      values:
                      - nginx-frontend
                topologyKey: failure-domain.beta.kubernetes.io/zone
    containers:
```

## Taints and Tolerations

Taints are applied to nodes to prevent Pods from being scheduled there, and tolerations are applied to Pods to allow them to be scheduled on tainted nodes.

| Effect | Description |
|---|---|
| NoSchedule | If a pod does not specify a toleration that matches the node taint(s), it will not be scheduled to run on the node. |
| PreferNoSchedule | If a pod does not specify a toleration that matches the node taint(s), the scheduler will attempt to avoid scheduling a pod on the node. |
| NoExecute | If a pod is already running on a node and a taint is added, if the pod does not match the taint, it will be evicted from the node. |

For example, to add a taint to a node named `node1`, use the following command:

```bash
kubectl taint nodes node1 gpu=:NoSchedule
```

This taint must be tolerated by a Pod before it can be scheduled on the node. If the Pod does not tolerate the taint, the scheduler will not schedule it on the node.

### Creating pods with Tolerations

To allow a Pod to run on a tainted node, add a `tolerations` field to the Pod specification. The toleration must match the taint key, value, and effect. If a Pod has a matching toleration, it can be scheduled on the node even when the node has the taint.

## ClusterRole aggregation

ClusterRole aggregation lets you create new ClusterRoles that are automatically merged into an existing aggregate ClusterRole, such as `admin`.

```yaml
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
    name: admin
...
aggregationRule:
    clusterRoleSelectors:
        - matchLabels:
            rbac.authorization.k8s.io/aggregate-to-admin: 'true'
```

A ClusterRole with the label `rbac.authorization.k8s.io/aggregate-to-admin: "true"` will be automatically aggregated into the `admin` ClusterRole. For example:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
    name: aggregate-example-admin
    labels:
        rbac.authorization.k8s.io/aggregate-to-admin: "true"
rules:
- apiGroups: ["newapi"]
  resources: ["newresource"]
  verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
```

Because the new ClusterRole has the matching aggregation label, its rules are merged into the `admin` ClusterRole, so the aggregated permissions are applied when rules are evaluated.

