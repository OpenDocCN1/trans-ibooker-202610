Next, you can examine the contents that have been installed within the Kind cluster. If you list the namespaces using `kubectl get ns`, you’ll note two new namespaces: `cert-manager` and `k8ssandra-operator`. As you may suspect, K8ssandra is using the same Cert Manager project as Pulsar, as described in Chapter 8. Let’s examine the contents of the `k8ssandra-operator` namespace, which are summarized in Figure 6-2 along with related K8ssandra CRDs. If you examine the workloads, you’ll notice that two Deployments have been created, one for the K8ssandra Operator, and one for Cass Operator.

![](bbox=[0.14, 0.33, 0.86, 0.82])

Figure 6-2. K8ssandra Operator Architecture