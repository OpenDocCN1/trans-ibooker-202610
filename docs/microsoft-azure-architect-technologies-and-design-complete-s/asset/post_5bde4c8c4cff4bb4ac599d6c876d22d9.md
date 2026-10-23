
# EXERCISE 4.17 (continued)

FIGURE 4.67 Checking the application on an AKS cluster using the Kubernetes command-line tool kubectl

```
PS Azure:\> kubectl get service csharpguitar-aks --watch
NAME              TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
csharpguitar-aks  ClusterIP   10.0.248.6              80/TCP    15m
```

+   9. Once you complete the exercise, or any exercise for that matter, you might consider removing the resources you created so that the cost is minimized.

That wasn’t too hard, but realize that the exercise is just scratching the surface, and there is so much more to it. There will be some books written about this at some point if there haven’t been already. To explain a bit more about what you just did, I start with the Azure CLI commands. The first one, az aks create, is the one that created the AKS cluster. The second, az aks get-credentials, retrieved and stored the credentials of the AKS cluster so that I would then be able to capture information using kubectl and perform other administrative work on the cluster. The Kubernetes command-line tool kubectl is installed by default when using Azure Shell. A reason I decided to use Azure Shell was to avoid the complexities of installing Azure CLI and kubectl on my workstation. Now that you have created an AKS cluster, developed an application, and deployed it to the cluster, read on to learn a bit about maintaining and scaling the AKS cluster.

## Maintaining and Scaling

Since you know that VMSS is used behind the scenes of AKS and you know the roles and responsibilities that come with IaaS, you will recognize that you must manage any OS upgrades, the OS patching, the version of Kubernetes, and of course the application running on the nodes. The details to specifically perform these updates will not be covered here; instead, only the concepts will be explained. When an update to Kubernetes is initiated, it is rolled out using the concept of cordoning and draining. This is done to minimize the impact the update may have on the IT solution. The first action that is triggered from the controller of the cluster master is to cordon one of the nodes and target it for upgrade. There is a waiting period after the node is identified so that any request running on the node can complete; this is considered draining. Once the node is no longer being used, the upgrade proceeds. Once complete, the controller places the node back into the pool and takes out another; the process is repeated until all nodes are upgraded. It is possible to upgrade just a node within the cluster or the entire cluster of nodes. In the Azure Portal on the AKS blade, you will find a navigation link named Upgrade, which will upgrade the entire cluster. Click the Node pools link on the AKS blade, and you will find an option to upgrade each node. Upgrading an application running on the AKS cluster has many available options; you can perform it manually, or you can choose any of the deployment options found by clicking the Deployment Center navigation menu item on the AKS blade. Azure DevOps Repos, GitHub, Bitbucket Cloud, and External Git are all options (as shown in Figure 4.61). Each of those implement CI/CD, which is very helpful when it comes to managing, developing, testing, and deploying application code (more on that in Chapter 8). For upgrading the OS version you have provisioned for AKS, you can find the VMSS instance that was created when you created the AKS cluster. Open the virtual machine scale set in the Azure Portal and navigate to the blade; the name should be prefixed with aks-. On the VMSS blade, click Instances, and then click the instance. The process for upgrading the instances running your AKS cluster is the same as you would do for any other VMSS implementation.

There are two methods for scaling: manual or automated. Manual scaling isn’t something you should consider when running a live production application. This kind of scaling capability is most useful during testing where you can learn how your application responds to scaling. You can scale from a pod or node perspective. To manually scale a pod, you can use the Kubernetes command-line kubectl. First use it to get a list of pods so you can see what you have; then once you identify the pod, scale that pod to, for example, five replicas or instances.

```
kubectl get pods
kubectl scale --replicas=5 <podName>
```

To manually scale a node to, for example, three instances, you can execute the following Azure CLI command from either Azure Cloud Shell or from a workstation with Azure CLI configured:

```
az aks scale --resource-group <name> --name <name> --node-count 3
```

From an autoscaling perspective, Kubernetes provides a service called Horizontal Pod Autoscale (HPA), which monitors the resource demand and scales the number of replicas/instances when required. This service does work with AKS but requires an optional component named Metrics Service for Kubernetes 1.8+. The HPA checks the Metric API exposed from the API server, which is part of the cluster master, every 30 seconds. If the metric threshold has been breached, then the scale out is managed by the controller. To check the scale metrics, you can use the Kubernetes command-line kubectl to check, using this command:

```
kubectl get hpa
```

To set the metric threshold, if the previous command doesn’t return any configurations, use the following:

```
kubectl autoscale deployment <podName> --cpu-percent=70 --min=2 --max=15
```

This command will add a pod when the average CPU across all existing pods exceeds 70%. The rule will increase up to a maximum of 15 pods, and when the CPU usage across the pods falls below 70%, the autoscaler will decrease one- by one to a minimum of two instances.

Also, you can autoscale a node (the VM on which the pods are running). Although there are some capabilities in the Azure Portal to scale out the number of VMs in the AKS cluster (it is a VMSS product after all), this is not recommended as it will get the AKS cluster controller and etcd out of sync. You can scale a VMSS using PowerShell, Azure CLI, or a supported client. This also is not recommended. It is recommended to always configure scaling or to manually scale using the Azure CLI Kubernetes component, which is accessed via Azure CLI and always followed by aks, for example az aks. The script that follows is an example of how to scale a node:

```
az aks update --resource-group <name> --name <name> \
  --update-cluster-autoscaler --min-count 2 --max-count 15
```

Having used the Azure CLI Kubernetes component, as recommended, the AKS cluster is set to have a minimum of two nodes and a maximum of 15. You might be asking yourself, what are the thresholds? Well, in practice, HPA and a node scaler service called *Cluster autoscaler* are used alongside each other. The cluster autoscaler will check the same metrics that were set for the pod every 10 seconds versus 30 seconds like HPA does for the pod. In a similar technique, HPA takes by increasing the number of pods based on the metrics. The cluster autoscaler will focus on the nodes (aka the VM) and adjust them appropriately to match the needs of the pod. You must be running Kubernetes version 1.10.x to get this cluster autoscale feature.

The final topic covered here is one that will handle a burst. I have mentioned the burst-ing concept a few times in regard to hybrid networks and HPC. That capability is also available when running AKS. In some scenarios, when a rapid burst occurs, there may be some delay with the provisioning of new pods as they wait for the scheduler and controller to get them rolled out; the requested increase in capacity simply hangs in a waiting state until they are added into the cluster. When the burst is unexpected and the number of nodes and pods exceeds the upper limit of your configuration, additional nodes simply are not allowed to be scaled out further. The way to handle this burst a bit better is to use *virtual nodes* and our friend ACI. Virtual nodes and their configuration into AKS is an advanced feature and is mentioned here only for your information. You can find more details here:

docs.microsoft.com/en-us/azure/aks/concepts-scale

## Cloud Services

Cloud Services were Microsoft’s first PaaS offering. At the time, Cloud Services offered some good capabilities like the all-powerful autoscaling feature. The problem with Cloud Services was the barrier of entry was high. To deploy an application to Cloud Services, the application needed to be reconfigured to run on that environment. This meant existing ASP.NET and ASP.NET MVC applications couldn’t simply be deployed to and run from Cloud Services. The application code needed to be migrated to a Cloud Services project in
