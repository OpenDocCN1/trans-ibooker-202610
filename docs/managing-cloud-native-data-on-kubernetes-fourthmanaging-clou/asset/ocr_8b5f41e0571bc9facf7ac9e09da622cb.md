through a portion of this guide here in order to get an understanding of the operator’s contents and how it works.

# EXAMPLES REQUIRE KUBERNETES CLUSTERS WITH MORE RESOURCES

The examples in previous chapters have not required a large amount of compute resources, and we encouraged you to run them on local distributions such as Kind or K3s. Starting with this chapter, the examples become more complex and may require more resources than you have available on your desktop or laptop. For these cases we will provide references to documentation or scripts for creating Kubernetes clusters with sufficient resources.

# Installing the Vitess Operator

You can find the source code used in this section at Vitess Operator Example. The files are copied for convenience from their original source in the Vitess GitHub repo. First, install the operator using the provided configuration file:

```
kubectl apply -f operator.yaml
  customresourcedefinition.apiextensions.k8s.io/
  etcdlockservers.planetscale.com created
  ...
```

As you’ll see in the output of the kubectl apply command, this configuration creates several CRDs, as well as a deployment managing a single instance of the operator. Figure 5-5 shows many of the elements you’ve just installed, in order to highlight a few interesting details will not be obvious at first glance:

+   - The operator contains a controller corresponding to each CRD. If you’re interested in seeing what this looks like in the operator source code in Go, compare the controller implementations with the custom resource specifications that are used to generate the CRD configurations introduced in Custom Resources. See more about building operators below.