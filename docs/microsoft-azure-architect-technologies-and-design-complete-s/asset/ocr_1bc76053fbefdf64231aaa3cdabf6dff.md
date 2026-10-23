# EXERCISE 4.17 (continued)

FIGURE 4.67 Checking the application on an AKS cluster using the Kubernetes command-line tool kubectl

```
PS Azure:\> kubectl get service csharpguitar-aks --watch
NAME              TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
csharpguitar-aks  ClusterIP   10.0.248.6              80/TCP    15m
```

+   9. Once you complete the exercise, or any exercise for that matter, you might consider removing the resources you created so that the cost is minimized.

That wasn’t too hard, but realize that the exercise is just scratching the surface, and there is so much more to it. There will be some books written about this at some point if there haven’t been already. To explain a bit more about what you just did, I start with the Azure CLI commands. The first one, az aks create, is the one that created the AKS cluster. The second, az aks get-credentials, retrieved and stored the credentials of the AKS cluster so that I would then be able to capture information using kubectl and perform other administrative work on the cluster. The Kubernetes command-line tool kubectl is installed by default when using Azure Shell. A reason I decided to use Azure Shell was to avoid the complexities of installing Azure CLI and kubectl on my workstation. Now that you have created an AKS cluster, developed an application, and deployed it to the cluster, read on to learn a bit about maintaining and scaling the AKS cluster.

# Maintaining and Scaling

Since you know that VMSS is used behind the scenes of AKS and you know the roles and responsibilities that come with IaaS, you will recognize that you must manage any OS upgrades, the OS patching, the version of Kubernetes, and of course the application running on the nodes. The details to specifically perform these updates will not be covered here; instead, only the concepts will be explained. When an update to Kubernetes is initiated, it is rolled out using the concept of cordoning and draining. This is done to minimize the impact the update may have on the IT solution. The first action that is triggered from the controller of the cluster master is to cordon one of the nodes and target it for upgrade. There is a waiting period after the node is identified so that any request running on the node can complete; this is considered draining. Once the node is no longer being used, the upgrade proceeds. Once complete, the controller places the node back into the pool and takes out another; the process is repeated until all nodes are upgraded. It is possible to upgrade just a node within the cluster or the entire cluster of nodes. In the Azure Portal on the AKS blade, you will find a navigation link named Upgrade, which will upgrade the entire cluster. Click the Node pools link on the AKS blade, and you will find an option