The next step is to point the `kubectl` tool to your new cluster:

```
az aks get-credentials --resource-group netcode-projects --name salesorder-cluster
```

`kubectl` is the main tool for interfacing with Kubernetes via the command line.

We can verify this is now working against our cluster by asking it to list our nodes:

```
kubectl get nodes
```

You should get a list of three nodes, all ready to go. The next thing we'll need to do is specify the Docker image to use in the cluster; we can do this by creating a Kubernetes deployment file (in YAML!).

# # Kubernetes deployment

One of the huge advantages of modern deployment techniques is that you no longer need to have someone in the company who *does the deployment*. I remember from my early professional career that you would either manually deploy software to a live environment yourself or know *someone in the office* that could do that. Needless to say, this technique was fraught with danger! Type in the wrong command and you may deploy the wrong software, or deploy the right software incorrectly or incompletely; it made diagnosing issues on-site extremely difficult because you were never quite sure what was there.

These days, there are many tools to help with this; however, Kubernetes (and the container ecosystem in general) is built with automated deployments as part of how you use it. As a result, in order to have anything deployed to Kubernetes, you must create a script (in either YAML or JSON) that tells Kubernetes what to deploy and how.

YAML is white-space sensitive, that is, the indentation levels matter.

Let's start by creating our YAML file:

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
```