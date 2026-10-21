
If you are installing Application Infrastructure with Helm, notice that Helm is not going to check for the health of these components, it is just doing the installation. It is quite common nowadays to find another alternative to install components in a Kubernetes cluster, called Operators. Usually associated with Application Infrastructure, you can find more active components that will install and monitor the components that were installed. One example of these Operators is the Zalando PostgreSQL Operator that you can find here: https://github.com/zalando/postgres-operator .

In general, Kubernetes Operators try to encapsulate the operational tasks associated with a specific component, in this case, PostgreSQL. We will cover the Operator Pattern in more detail in Chapter X. While using Operators might add more features on top of installing a given component, you still need to maintain the component and the operator itself now.

Regarding the Application Infrastructure that you and your teams decide to use if you are planning to run these components inside your cluster, plan accordingly to have the right expertise in house to manage, maintain and scale these extra components.

In the following section, we will look at how we can tackle these challenges by looking at an Open Source project that aims to simplify the provisioning of Cloud resources and application infrastructure by using a declarative approach.

## 4.2 Defining Infrastructure in a declarative way using Crossplane

Using Helm to install application infrastructure components inside Kubernetes is far from ideal for large applications and user-facing environments, as the complexity of maintaining these components and their requirements such as advanced storage configurations might become too complex to handle for your teams.

Cloud Providers do a fantastic job at allowing us to provision infrastructure, but they all rely on cloud provider-specific tools which are outside of the realms of Kubernetes.

In this section, we are going to look at an alternative tool; a CNCF project called Crossplane (https://crossplane.io), which uses the Kubernetes APIs and extension points to enable users to provision real infrastructure in a declarative way, using the Kubernetes APIs. Crossplane relies on the Kubernetes APIs to support multiple Cloud Providers, this also means that it integrates nicely with all the existing Kubernetes tooling.

By understanding how Crossplane works and how it can be extended you can build a multi-cloud approach to build and deploy your Cloud Native applications into different providers without worrying about getting locked-in. Because Crossplane uses the same declarative approach as Kubernetes, you will be able to create your own high level abstractions about the applications that you are trying to deploy and maintain.

### 4.2.1 Crossplane Providers

Crossplane extends Kubernetes by installing a set of components called “Providers” which are in charge of understanding and interacting with cloud provider-specific services to provision these components for us.

Figure 4.5 Crossplane installed with GCP and AWS Providers

By installing Crossplane providers you are extending the Kubernetes APIs functionality to provision external resources such as Databases, Message Brokers, Buckets and other Cloud Resources that will live outside of your Kubernetes cluster, but inside the Cloud Provider realm. There are several Crossplane providers covering the major cloud providers such as GCP, AWS and Azure. You can find these Crossplane providers in the Crossplane Github’s organization: https://github.com/crossplane/

Once a Crossplane Provider is installed you can create provider-specific resources in a declarative way, which means that you can create a Kubernetes Resource, apply it with `kubectl apply -f`, package these definitions in Helm Charts or use Environment Pipelines storing these resources in a Git repository.

Provisioning cloud-specific resources relying on the Kubernetes APIs is a big step forward but Crossplane doesn’t stop there. If you go to look at the details of what it takes to provision a database in any major cloud provider you will realize that provisioning the component is just one of the tasks involved in getting the component ready to be used. For connecting to these provisioned components you will need network and security configurations, as well as user credentials.

### 4.2.2 Crossplane Compositions

Crossplane aims to serve two different Personas: "Platform teams" and "Application teams". While Platform teams are Cloud Providers experts that understand how to provision cloud provider-specific components, *Application* teams know the application requirements and understand what is required from the Application Infrastructure perspective. The interesting thing about this approach is that when using Crossplane, Platform teams can define these complex configurations for a specific Cloud Provider and expose simplified interfaces for Application teams.

In real-life scenarios, it is rare to just create a single component, for example, if we want to provision a Database instance application teams will also require the correct network and security configurations to be able to access the newly created instance. Being able to compose and wire up together several components is a very convenient feature and for achieving these abstractions and simplified interfaces Crossplane introduced two concepts "*Composite Resource Definitions*" and "*Composite Resources*".

![](img/2ab98a5ff1ec073700d902ba95287fd6_110_0.png)

Figure 4.6 Resource Composition abstraction by Crossplane Composite Resources

The previous diagram shows how you can use Crossplane *Composite Resources* to define abstractions for different Cloud Providers. The *Platform team* might be very knowledgeable in Google Cloud or Azure so they will be in charge of defining which Resources they want to wire up together for a specific application. The *Application team* have a simple Resource interface to request the Resource that they are interested in. But as usual, abstractions are complicated and good to show who is responsible for what, but let’s look at a concrete example to understand the power of Crossplane Compositions.

## Figure 4.7 Provisioning a PostgreSQL instance in Google Cloud with Crossplane Compositions

Figure 4.x shows how the application team can create a simple PostgreSQL resource to a provision in Google Cloud a CloudSQLInstance plus a Network configuration and a Bucket. The application team is not really interested in what resources are created or even in which Cloud Provider they were created, they are only interested in having a PostgreSQL instance to connect their applications to.

This takes us to the “Secret” box in the figure, which represents a Kubernetes Secret that Crossplane will create for our application/services Pods to connect to the provisioned resources. Crossplane creates this Kubernetes Secret with all the details required by our applications to connect to the newly created resources (or just with the one relevant for the application). This Secret typically contains URLs, usernames, passwords, certificates or anything required for your applications to connect. Platform teams define what is going to be included in the secret when defining the CompositeResources. In the following sections, when we add real infrastructure to our Conference Platform we will explore how these CompositeResources look and how they can be applied to create all the components that our applications need.

Another really important feature of Crossplane is that you can package these compositions into an OCI image (a container) and share that as any other docker image. Once you install these Crossplane packages with configurations, the Platform team can create these simplified resources without the need of understanding or knowing which provider is available to them. This clear separation of concerns (requesting application
