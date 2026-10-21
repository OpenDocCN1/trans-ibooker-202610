performance, logging, upgrading the cluster, applying patches, and so on. To reduce this toil, Azure offers a managed solution called *Azure Kubernetes Service* or *AKS*.

AKS makes the process of deploying and managing a Kubernetes cluster in Azure a cakewalk. An Azure-managed Kubernetes solution eliminates a big chunk of the Kubernetes management process, and all you are left with is your application to manage. AKS offers several advantages, including the following:

+   - Health monitoring of clusters is fully managed by Azure.
+   - Maintenance tasks of the underlying nodes are the responsibility of Azure.
+   - The control plane is managed and maintained by Azure, and you as a user only have to manage the agent nodes.
+   - AKS lets you integrate with Azure Active Directory and the Kubernetes RBAC API for security and access control.
+   - All logs for your application and cluster are stored in the Azure Log Analytics workspace.
+   - The Horizontal Pod Autoscaler (HPA) and Cluster Autoscaler are seamless to use in respect to setting up a Kubernetes cluster from scratch and taking care of underlying resource permissions.
+   - AKS offers seamless support for the storage volume.

Now that you understand the advantages of AKS, let’s create an AKS cluster. There are two simple ways to do this: use the Azure portal (the manual approach) or use Terraform (the automated approach).

You can navigate through the Azure console and start deploying your AKS cluster by clicking the Add button (Figure 5-2) and then following the instructions as they appear.

![](bbox=[0.19, 0.66, 0.81, 0.76])

Figure 5-2. Clicking the Add button to create an AKS cluster

Alternatively, in Terraform you can go to the AKS directory of the repo /cloud_native_azure/Chapter5/AKS and execute the following command:

```
$ terraform apply
```