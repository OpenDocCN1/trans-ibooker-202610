```
apiVersion: configmanagement.gke.io/v1
metadata:
  name: non-testers
spec:
  selector:
    matchExpressions:
      - key: is-rollout-tester
        operator: DoesNotExist
---
kind: ClusterSelector
apiVersion: configmanagement.gke.io/v1
metadata:
  name: country-us
spec:
  selector:
    matchLabels:
      country: us
```

These selectors are then used to control deployments of new versions of applications, or to only deploy certain applications in certain regions. For example, only restaurants in the United States have drive-thrus. Therefore, the drive-thru management application only needs to be deployed to the "country-us" clusters. The company has also decided to have certain selected stores be test beds of new software, as indicated by the "is-rollout-tester" flag on their cluster. An example deployment for an application is included here. However, some portions of the template have been removed as they are identical between the two examples:

```
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: foh-engine
  annotations:
    configmanagement.gke.io/cluster-selector: rollout-testers
spec:
  template:
    spec:
      containers:
        - name: engine
          image: gcr.io/ambiguous-rock/foh/engine:v2.1.0
          imagePullPolicy: IfNotPresent
---
apiVersion: apps/v1
kind: Deployment
```