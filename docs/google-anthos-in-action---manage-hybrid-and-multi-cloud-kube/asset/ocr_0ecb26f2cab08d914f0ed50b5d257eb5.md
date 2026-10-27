The repo generally looks as follows:

```
<Git Repo Parent>
├─ bin
├─ policy_directory
│  ├─ namespaces
│  │  ├─ rbac.yaml
│  │  ├─ central
│  │  │  ├─ namespace.yaml
│  │  │  ├─ database_location_config.yaml
│  │  ├─ applications
│  │  │  ├─ service_account.yaml
│  │  │  ├─ website
│  │  │  │  ├─ namespace.yaml
│  │  │  │  ├─ hierarchy.yaml
│  │  │  │  ├─ repo-sync.yaml
│  │  │  ├─ inventory
│  │  │  │  ├─ namespace.yaml
│  │  │  │  ├─ hierarchy.yaml
│  │  │  │  ├─ repo-sync.yaml
│  │  ├─ village-linen-ac15e6
│  │  │  ├─ namespace.yaml
│  │  │  ├─ hierarchy.yaml
│  │  │  ├─ repo-sync.yaml
│  ├─ cluster
│  │  ├─ rbac.yaml
│  │  ├─ hierarchy.yaml
│  │  ├─ constraints.yaml
│  ├─ clusterregistry
│  │  ├─ data center.yaml
│  │  ├─ cloud.yaml
│  │  ├─ selectors.yaml
│  ├─ system
│     └─ repo.yaml
```

Starting from the bottom, we have a definition for the repo that contains the current version of the policies. In the clusterregistry directory, we have cluster definitions for the cluster in the local data center, as well as the cluster in GCP. We also have selectors defined for each of these clusters so that we can restrict resources in the namespaces directory. The cloud cluster declaration and selector, for example, are:

```
kind: Cluster
apiVersion: clusterregistry.k8s.io/v1alpha1
metadata:
```