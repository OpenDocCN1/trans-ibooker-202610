The main idea behind this component is to make sure that nobody will change the environment configuration by manually interacting with the cluster. This component is the only one allowed to change the environment configuration, deploy new services, upgrade services versions or remove services from the environment.

For an Environment Pipeline to work, the following two considerations need to be met:

+   - The repository containing the desired state for the environment needs to have all the necessary configurations to create and configure the environment successfully.
+   - The Kubernetes Cluster where the environment will run needs to be configured with the correct credentials for allowing the state to be changed by the pipelines.

The term Environment Pipeline makes reference to the fact that each Environment will have a pipeline associated with it. As having multiple environments is usually required (development, staging, production) for delivering applications, each will have a pipeline in charge of deploying and upgrading the components that are running in them. By using this approach, promoting services between different environments is achieved by sending Pull Requests/Change Requests to the environment’s repository and the pipeline will take care of reflecting the changes into the target cluster.

# ENVIRONMENT PIPELINE REQUIREMENTS AND DIFFERENT APPROACHES

So what are the contents of these Environment’s repositories? In the Kubernetes world, an environment can be a namespace inside a Kubernetes cluster or a Kubernetes cluster itself. Let’s start with the most straightforward option, a "Kubernetes namespace". As you will see in the figure that follows, the contents of the Environment Repository is just the definition of which services need to be present in the environment, the pipeline then can just apply these Kubernetes manifests to the target namespace.

The following figure shows 3 different approaches that you can use to apply configurations files to a Kubernetes Cluster. Notice that the three options all include an `environment-pipeline.yaml` file with the definition of the tasks that needs to be executed.

![](bbox=[0.13, 0.665, 0.88, 0.795])

Figure 3.X Three different approaches for defining environments' configurations

The first option (Simple layout) is just to store all the Kubernetes YAML files into a Git repository and then the Environment Pipeline will just use `kubectl apply -f *` against the configured cluster. While this approach is simple, there is one big drawback, if you have your