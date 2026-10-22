# NAMESPACES AND KUBERNETES RESOURCE SCOPE

If you have chosen to install your Helm release in a namespace, you’ll need to specify the namespace on most of your `kubectl get` commands in order to see the created resources. The exception is `kubectl get pv`, because PersistentVolumes are one of the Kubernetes resources that are not namespaced, that is, they can be used by pods in any namespace. To learn more about which Kubernetes resources in your cluster are namespaced and which are not, run the command `kubectl api-resources`.

# How Helm Works

Did you wonder what happened when you executed the `helm install` command with a provided values file? To understand what’s going on, let’s take a look at the contents of a Helm chart, as shown in Figure 4-2. As we discuss these contents, it will also be helpful to look at the source code of the MySQL Helm chart you just installed.

![](bbox=[0.15, 0.48, 0.85, 0.8])

*Figure 4-2. Customizing a Helm release using a values.yaml file*

Looking at the contents of a Helm chart, you’ll notice the following: