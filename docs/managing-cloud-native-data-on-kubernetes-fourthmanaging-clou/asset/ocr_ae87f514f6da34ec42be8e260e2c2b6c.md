By comparing the configuration script with Figure 5-6, you can make several observations about this simple VitessCluster. First, the top level configuration allows you to specify the name of the cluster and the container images that will be used for the various components:

```
apiVersion: planetscale.com/v2
kind: VitessCluster
metadata:
  name: example
spec:
  images:
    vtctld: vitess/lite:v12.0.0
    ...
```

Next, the VitessCluster configuration provides a definition of the VitessCell zone1. The values provided for gateway specify a single VTGate instance to be allocated for this cell, with specific compute resource limits.

```
cells:
- name: zone1
  gateway:
    authentication:
      static:
        secret:
          name: example-cluster-config
          key: users.json
    replicas: 1
    resources:
      ...
```

The Vitess Operator uses this information to create a VTGate Deployment prefixed with example-zone1-vtgate containing a single replica, and a Service that provides access. The access credentials for the VTGate instance are provided in the example-cluster-config Secret. This Secret is used to secure other configuration values, as you’ll see below.

The next section of the VitessCluster configuration specifies the creation of a single vtctld instance (aka “dashboard”) with permission to control zone1. The Vitess Operator uses this information to create a Deployment to manage the dashboard using the specified resource limits, and a Service to provide access to the VTGate.