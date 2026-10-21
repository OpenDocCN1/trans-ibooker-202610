Kubernetes YAML files for each service in the service repository, then the environment repository will have these files duplicated and they can go out of sync. Imagine if you have several environments you will need to maintain all the copies in sync and it might become really challenging.

The second option (Helm layout) is a bit more elaborate, now we are using Helm to define the state of the cluster. You can use Helm dependencies to create a parent chart that will include as dependencies all the services that should be present in the environment. If you do so, the environment pipeline can use `helm update` to apply the chart into a cluster. Something that I don't like about this approach is that you create one Helm release per change and there are no separate releases for each service. The prerequisite for this approach is to have every service package as a Helm chart available for the environment to fetch.

The third option is to use a project called `helmfile` (https://github.com/robol1/helmfile) which was designed for this very specific purpose, to define environment configurations. A `helmfile` allows you to declaratively define what Helm releases need to be present in our cluster. This Helm releases will be created when we run `helmfile sync` having defined a `helmfile` containing the helm releases that we want to have in the cluster.

No matter if you use any of these approaches or other tools to do this, the expectation is clear. You have a repository with the configuration (usually one repository per environment) and a pipeline will be in charge of pickup the configuration and using a tool to apply it to a cluster.

It is common to have several environments (staging, qa, production), even allowing teams to create their own environments on-demand for running tests or day to day development tasks.

If you use the "one environment per namespace" approach, it is common to have a separate git repository for each environment, as it helps to keep access to environments isolated and secure. This approach is simple, but it doesn't provide enough isolation on the Kubernetes Cluster, as Kubernetes Namespaces were designed for logical partitioning of the cluster and in this case, the Staging Environment will be sharing with the Production environment the cluster resources.

![](bbox=[0.13, 0.62, 0.68, 0.78])

One strategy is to use namespaces for different environments. While this simplifies the configurations required for the pipelines to deploy services to different environments, namespaces doesn't have strong isolation guarantees.

Figure 3.X One Environment per Kubernetes Namespace

An alternative approach can be to use an entirely new cluster for each environment. The main difference is isolation and access control. By having a cluster per environment you can be more strict in defining who and which components can deploy and upgrade things in these environments and have different hardware configurations for each cluster, such as multi-region setups and other