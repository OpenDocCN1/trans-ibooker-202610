![](bbox=[0.195, 0.108, 0.813, 0.233])

To configure the Connect Gateway and connect your Anthos cluster follow the steps described in the Google documentation:
https://cloud.google.com/anthos/multicluster-management/gateway/setup

Once the Connect gateway is configured and Anthos servers are registered check if they are visible in the hub by running:

```
gcloud container hub memberships list
```

In this case we see two clusters are registered. One is a GKE on VMware the other is a GCP GKE cluster.

```
NAME                EXTERNAL_ID
my-vmware-cluster   0192893d-ee0d-11e9-9c03-42010a8001c1
my-gke-cluster      f0e2ea35-ee0c-11e9-be79-42010a8400c2
```

Let us define the following step to deploy an application defined in myapp.yaml manifest.

```
steps:
- name: 'gcr.io/cloud-builders/gcloud'
  entrypoint: /bin/sh
  id: Deploy to Anthos cluster on VMware
  args:
  - '-c'
  - |
    set -x && \
    export KUBECONFIG="$(pwd)/gateway-kubeconfig" && \
    gcloud beta container hub memberships get-credentials my-vmwa
    kubectl --kubeconfig gateway-kubeconfig apply -f myapp.yaml
```

As we see in this case the gateway kubconfig is used rather than the cluster kubeconfig itself. The request will be sent to the gateway and then the