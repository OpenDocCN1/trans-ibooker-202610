
## 4
## Multi-Cloud Infrastructure
## This chapter covers

- Define and manage the infrastructure required by your applications
- The challenges of managing your own application infrastructure components
- The Kubernetes way to deal with infrastructure (Crossplane)

In the previous chapters, we installed a walking skeleton, we understood how to build each separate component using Service Pipelines and how to deploy them into different environments using Environment pipelines. We are now faced with a big challenge: dealing with infrastructure, meaning creating environments where our applications will run. It is normal and expected that these applications require other components to be there to work correctly, such as Databases, Message Brokers, Identity Management solutions, Email Servers, etc. While there are several tools out there focused on how to automate the installation or provisioning of these components for On-Premises setups and in different cloud providers, in this chapter, we will focus on just one that does it in a Kubernetes way. This chapter is divided into three main sections:

- The challenges of dealing with infrastructure
- How to deal with infrastructure leveraging Kubernetes constructs
- How to provision infrastructure for our walking skeleton using Crossplane.io

After covering these sections we will talk about building platforms on top of Kubernetes, a challenge that sooner or later will come knocking on your door.

## 4.1 The challenges of managing with Infrastructure in Kubernetes

When you design applications like the walking skeleton introduced in Chapter 1, you are faced with specific challenges that are not core to achieving your business goals. Installing, configuring and maintaining Application Infrastructure components that support our application’s services is a big task that needs to be planned carefully by the right teams with the right expertise.

These components can be classified as Application Infrastructure, which usually involve third-party components that are not going to be developed in house, such as Databases, Message Brokers, Identity Management solutions, etc. A big reason behind the success of modern Cloud Providers is that they are great at taking care of provisioning and maintaining these components and allowing your development teams to focus on building the core features of our applications, the ones that bring value to the business.

It is important to distinguish between Application Infrastructure and Hardware Infrastructure, which is also needed. In general, I am assuming that for Public Clouds offerings, all hardware related topics are solved by the provider itself. For On-Prem scenarios, it is most likely that you have a specialized team taking care of the Hardware (removing, adding and maintaining hardware as needed).

It is quite common to rely on Cloud Provider services to provision Application Infrastructure, there are a lot of advantages in doing so, such as pay as you use the services, easy provisioning at scale and automated maintenance. But at that point, you heavily rely on provider-specific ways of doing things and their tools. The moment you create a database or a message broker in a cloud provider you are jumping outside of the realms of Kubernetes. At this point you are depending on their tools, their automation mechanisms and you are creating a strong dependency of your business with the specific Cloud Provider.

Let’s take a look at the challenges associated with provisioning and maintaining application infrastructure, so your teams can plan ahead and choose the right tool for the job:

- **Configuring components to scale:** each component will require different expertise to be configured (Database Administrators for databases and Message Broker experts) and a deep understanding of how our application’s services will use it, as well as the hardware available. These configurations need to be versioned and monitored closely, so new environments can be created quickly to reproduce issues or just to test new versions of our application.
- **Maintaining components in the long run:** components such as databases and message brokers are constantly released and patched to improve performance and security, this pushes the operations teams to make sure that they can upgrade to newer versions, keep all the data safe without bringing down the entire application. This requires a lot of coordination and impact analysis between the teams involved with these components and services.
- **Cloud Provider services affect our multi-cloud strategy:** if we rely on cloud-specific application infrastructure and tools, we need to find a way to enable developers to create and provision their components for developing and testing their services. We need a way to abstract how infrastructure is provisioned to enable applications to define what pieces of infrastructure they need without relying directly on cloud-specific tools.

It is interesting to see that we had these challenges even before having distributed applications, and configuration and provisioning architectural components have always been hard and usually far away from developers. Cloud Providers are doing a fantastic job by bringing these topics closer to developers so they can be more autonomous and iterate faster. Unfortunately, when working with Kubernetes we have more options that we need to consider carefully to make sure that we understand the trade-offs. The next section covers how we can manage our Application Infrastructure inside Kubernetes; while this is usually not recommended, it can be practical for some scenarios.

### 4.1.1 Managing your own Application Infrastructure

Application infrastructure has become an exciting arena. With the rise of containers, every developer can bootstrap a database or a message broker with a couple of commands, and for development purposes, this is usually enough. In the Kubernetes world, this translates to Helm Charts, which uses containers to configure and provision Databases (relational and NoSQL), message brokers, identity management solutions, etc. As we saw in chapter 2 you installed the walking skeleton application containing 4 services with a single command. The same can be done for application infrastructure components.

For example, you can run `helm install postgresql bitnami/postgresql` to install an instance of PostgreSQL in your cluster. Same with Redis, Kafka and RabbitMQ. The amount of Helm charts available today is amazing, and it is quite easy to think that installing a Helm Chart will be the way to go.

```
helm install postgresql bitnami/postgresql
```

As discussed in Chapter 2 (Dealing with application state), if we want to scale our services that keep state we will need to provision specialized components such as databases. Application developers will define which kind of database will suit best depending on the data that they need to store and how that data will be structured. For our walking skeleton, we will be provisioning a Redis NoSQL database for the Agenda Service and a PostgreSQL database for the Call for Proposals (C4P) Service.

![](img/2ab98a5ff1ec073700d902ba95287fd6_102_0.png)

Figure 4.1 Keeping service state using application infrastructure

The process of setting up these components inside your Kubernetes Cluster involves the following steps:

- Finding or creating the right Helm Chart for the component you want to bootstrap: for this case, PostgreSQL (https://github.com/bitnami/charts/tree/master/bitnami/postgresql) and Redis (https://github.com/bitnami/charts/tree/master/bitnami/redis) can be found in the Bitnami Helm Chart repository. If you cannot find a Helm Chart but you have a Docker Container for the component that you want to provision you can create your own chart after you define the basic Kubernetes constructs needed for the deployment.
- Research the Chart configurations and parameters that you will need to set up to accommodate your requirements. Each chart exposes a set of parameters that can be tuned for different use cases, check the chart website to understand what is available. Here you might want to include your Operations teams and for example, DBAs to check how the Databases needs to be configured. This will also require Kubernetes expertise to make sure that the components can work in HA (High Availability) mode inside Kubernetes.
- Install the chart into your Kubernetes Cluster, using `helm install`. By running `helm install` you are downloading a set of Kubernetes manifest (YAML files) that describe how your applications/services need to be deployed. Helm then will proceed to apply these YAML files into your cluster.
