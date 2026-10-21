This will generate an OCI container that you can push to Docker Hub or any other container registry. The name of the OCI container will be defined by the name of the Configuration resource that you have defined in the crossplane.yaml file. To push the configuration package to the registry you can also use the plugin:

```
kubectl crossplane push configuration salaboy/crossplane-fmtok8s-gcp:0.0.8
```

Notice that you need to have the right credentials to push to the target registry. If not specified it will use Docker Hub, where "salaboy" is my Docker Hub username.

If you really want to create a self-service platform, as the Google Cloud user interface but on top of Kubernetes you will need to build a User Interface that reads from the installed Configuration Packages the **CompositeResourceDefinitions** type Schema definitions. You can build parameterizable user screens where users can select the types of resources they want, enter the required parameters and create resources without the need of having `kubectl` installed in their environments.

# 4.4.3 Extending Crossplane with our custom Providers

Most of the examples and scenarios discussed in this chapter are targeting Cloud Providers, but Crossplane can be also used for On-Premises setups and extended to support other managed services besides the ones provided by AWS, Azure and Google. The abstractions and mechanisms provided by Crossplane are valuable in a wide range of scenarios where the resources that we want to provision and monitor are outside of the Kubernetes Cluster where our services are running.

If you want to extend Crossplane with your own Providers I recommend you to check the Provider Template (https://github.com/crossplane/provider-template) project that shows a simple example of how a Provider can be created and distributed. A Crossplane Provider consists of a Kubernetes Controller which will use credentials to connect to the remote Managed Service and then expose the resources that can be provisioned remotely as Kubernetes Resources.