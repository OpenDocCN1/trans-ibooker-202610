
+   - Input and Output mappings to share data between tasks
+   - Event triggers that allow you to listen for events that will trigger Pipelines or Tasks
+   - A Command-Line tool to easily interact with Tasks and Pipelines from your terminal
+   - A Simple Dashboard to monitor your pipelines and tasks executions.

![](img/2ab98a5ff1ec073700d902ba95287fd6_90_0.png)

Figure 3.X Tekton Dashboard a user interface to monitor your pipelines

Figure 3.x shows the community-driven Tekton Dashboard which you can use to visualize the execution of your pipelines. Remember that because Tekton was built to work on top of Kubernetes, you can monitor your pipelines using `kubectl` as with any other Kubernetes resource, but nothing beats a User Interface for less technical users.

But now, if you want to implement a Service Pipeline with Tekton, you will spend quite a bit of time defining Tasks, the Pipeline, how to map inputs and outputs, defining the right events listener for your Git repositories and then going more low-level into defining which docker images you will use for each Task. Creating and maintaining these pipelines and their associated resources can become a full-time job and for that Tekton launched an initiative to define a Catalog where Tasks (Pipelines and Resources are planned for future releases) can be shared, the Tekton Catalog: https://github.com/tektoncd/catalog

With the help of the Tekton Catalog, we can create pipelines that reference Tasks that have been defined in the catalog, hence we don’t need to worry about defining them. You can also visit https://hub.tekton.dev which allows you to search for Task definitions and provides you with detailed documentation about how to install and use these tasks in your pipelines.

![](img/2ab98a5ff1ec073700d902ba95287fd6_91_0.png)

Figure 3.X Tekton Hub a portal to share and reuse Tasks and Pipeline definitions

One good example to look at is the Helm Upgrade from Source Task which allows you to upgrade a Helm release based on the Helm Chart source. You can take a look at the task documentation here: https://hub.tekton.dev/tekton/task/helm-upgrade-from-source A Task like this one, can be really helpful to implement an environment Pipeline, where we define the content of the Environment with a Helm chart and then every time that we change the content of the chart we just upgrade the Helm Release with Tekton.

Tekton Hub and the Tekton Catalog allows you to reuse Tasks and Pipelines that had been created by a large community of users and companies. But you quickly realize that Tekton is not very opinionated on what you do with the pipelines that you build, hence there are loads of different options and choices that you will need to make when creating a Service or an Environment pipeline. Wouldn’t it be nice if all the defaults, conventions and best practices that are being used by tons of other people would be shared in a single tool? In the next section, we will take a look at Jenkins X and how it takes a holistic approach to solve CI/CD for Kubernetes.

## 3.3.2 Jenkins X: A one-stop-shop for CI/CD in Kubernetes

Jenkins X (http://jenkins-x.io) offers a whole package for doing CI/CD on top of Kubernetes. This goes from installing all the components that you need, for example, the Pipeline Engine (Tekton in this case) to the user interface to monitor the pipelines, artifact repositories and the best practices applied to convention-based pipelines. In theory, with Jenkins X, after you installed it you just need to import your services with a single command line and both Service and Environment Pipelines will be set up for your project. Jenkins X takes a holistic approach to solve CI/CD hence it is a complex

project to get your head around at first. This section aims to explain how Jenkins X solves and simplify the whole CI/CD process for Kubernetes projects, my recommendation is always to try the project on your own clusters, but it will require a whole book to explain how all the pieces fit together.

## JENKINS X ARCHITECTURE

If you are interested in trying out Jenkins X, you need to understand that Jenkins X is divided into two main components:

+   - **A command-line tool called `jx`**: You need to download the `jx` command-line from the http://jenkins-x.io site or use tools such as brew for Mac OSX or a package manager to install the command line locally.

+   - **A set of services and components installed into a Kubernetes Cluster:** By using the `jx` command-line tool you can install Jenkins X in an existing Kubernetes Cluster. You will probably install Jenkins X in a separate cluster where your applications are deployed.

The following figure shows the components that are installed by default when you install Jenkins X, no matter in which Cloud Provider you decide to install it.

![](img/2ab98a5ff1ec073700d902ba95287fd6_92_0.png)

When installing Jenkins X in your cluster, it does install all the required infrastructure for building your Cloud Native Services and manage your environments.

Figure 3.X Jenkins X Components installed on a Kubernetes Cluster

With the command-line tool called `jx` you can install Jenkins X into your cluster. In order to run Jenkins X you need a Kubernetes Cluster, to begin with, Jenkins X can create one for you. Jenkins X uses a tool called Terraform (https://www.terraform.io) to create the cluster and install the required components into different Cloud Providers. It also uses Kubernetes and Cloud Provider-specific services to make sure that your service and environment pipelines have everything they need to work. For example, if you are installing Jenkins X in Google Cloud Platform, it configures GCR (Google Container Registry) to be used to store the docker images produced by your service pipelines. In this section, we will look at what gets installed when you install Jenkins X in your cluster and the basic commands to go from zero to full CI/CD for your Services.

Jenkins X installs and configures all the components that are needed for your Service and Environment pipelines, making sure that components can register to git repositories for changes and the pipelines are automatically triggered when a change is pushed. Jenkins X also installed the pipeline engine (Tekton) and the repositories needed for the artifacts that are going to be created as part of the pipelines. Remember that you will need a place to store your binary artifacts (Nexus

is installed), your docker images (depending on the Cloud provider where you are installing Jenkins X a different container registry will be used, if you are in Google Cloud, GCR.io will be used) and also your Kubernetes manifest. Jenkins X relies on Helm for packaging and distributing Kubernetes manifests (YAML files) hence it installs Chart Museum into the cluster.

When you install Jenkins X in your Kubernetes Cluster, besides installing all the components that it needs, it will automatically create two environments: Staging and Production with their respective Environment Pipelines and the Git repositories containing the default configurations. As you can see in the following figure, your applications’ services will start in the `dev` environment, automatically promoted to the `staging` environment and finally reaching to the `production` environment, where users consume our services.

![](img/2ab98a5ff1ec073700d902ba95287fd6_93_0.png)

Figure 3.X Jenkins X pre-defined environments

Both pipelines will be associated with two separate namespaces, where the workloads for each environment will run. By default, no service pipeline is created, because there is no application Service defined yet.

As part of the installation process, Jenkins X creates two default environments called "staging" and "production", where automatic promotions are configured for the staging environment, meaning that whenever there is a new version of our services, the Environment Pipeline will be in charge of "promoting" (adding or upgrading the service) into the environment. The production environment requires manual promotion, this means that a user will be in charge of deciding when new versions of the services can be installed in the production environment.

So, far after just installing Jenkins X, we have two environments and default environment pipelines ready to go. You are now ready to start adding Services.

## YOUR PROJECTS AND JENKINS X

Once all the components needed by the pipelines are installed and configured to work together you can start "importing" your projects to Jenkins X.

Let’s take a look at how this will look for our Conference Platform application which was composed of four services. If we have four services, we need four service pipelines. Each of these pipelines will release each service independently. In Jenkins X, we can create these pipelines by
