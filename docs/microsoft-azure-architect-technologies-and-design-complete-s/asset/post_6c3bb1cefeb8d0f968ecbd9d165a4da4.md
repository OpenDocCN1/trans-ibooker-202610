
Directory so that users inside an internal network can use the same credentials to authenticate on a compute resource running on the Azure platform. The tool to use for this synchronization is called Azure AD Connect, which will be discussed in much more detail in the next chapter.

In addition to having detailed knowledge of AAD, to increase the probability of successful exam completion, it is necessary to know these Azure security features and technologies and their use cases:

- Azure AD Connect
- Custom domain names
- RBAC
- Key Vault
- Azure Active Directory Domain Services
- Managed Service Identity (MSI)
- Shared Access Signature (SAS)
- AAD conditional access policies
- AAD domain services
- Azure confidential computing
- Multifactor authentication
- Single sign-on (SSO)
- Security Center
- Encryption techniques, data-at-rest, data-in-transit
- Certificate authentication, SSL/TLS, certificate authority (CA)
- Forms, token, OAuth, JWT, and claims authentication techniques

This list contains not only products and features available on Azure but also technologies and models for securing application and resources. Knowing which type of authentication is best used in a given scenario is helpful when taking the exam. Additionally, having some practical experience with the tools improves your knowledge of the limits and constraints imposed by each, which leads to making good designs with specific products and features based on a given scenario. More detail, a few scenarios, and exercises are covered in later chapters.

## Networking

Using any of the Azure Infrastructure as a Service (IaaS) or Platform as a Service (PaaS) offerings, a network infrastructure is provided as part of the service. Azure VM, App Service, or Azure Functions is allocated an IP address (internal and external), and in most scenarios, the endpoint becomes immediately and globally accessible.

Keep these two topics in mind:

Cost: Although the network is provided as a service, that doesn’t mean it is free. Network attributes such as public IP addresses, inbound and outbound global/regional

IaaS can require more support and maintenance costs when compared to other Azure compute products. However, these costs may be justified because of missing features or constraints in the PaaS offerings.

## Azure App Service

Azure App Service is Microsoft’s PaaS offering. This means that in addition to the network infrastructure being the responsibility of the cloud provider, the operating system and its patching are no longer a concern to the customer. Azure App Service comes in four flavors.

- Azure Web Apps/WebJobs
- Azure App Service Web App for Containers
- Azure Mobile Apps
- Azure API Apps

There is one additional flavor—an enterprise-grade app known as the App Service Environment (ASE). ASE is an isolated Azure App Service tenant that belongs solely to the customer who creates it. There are some capabilities available only in ASE, for example, configuring cipher suite order. That specific example makes sense in the public App Service tenant because all customers running in that tenant may not want the cipher suite configured in that order.

Azure App Service offers both the Windows and Linux operating systems and supports a large majority of coding languages, both Microsoft and open source such as .NET Core, Java, Ruby, Node.js, and so on. All flavors of Azure App Service support built-in autoscaling, load balancing, integration with AAD and OAuth providers, and great deployment capabilities with integration with Git, DevOps (formerly Visual Studio Team Services [VSTS]), and GitHub.

From an Azure Web Apps perspective, use one of those source code repositories for hosting a custom web application or for running content management systems (CMSs) such as WordPress, Umbraco, Joomla!, or Drupal. The WebJobs service is used for longer-running workloads that don’t require an HTTP connection to remain connected from start to finish. Consider a WebJob instance to be something like a batch job or a small program that runs offline in the background that is triggered by a cron scheduler; these are referred to as a triggered WebJob. WebJobs can also be continuous/AlwaysOn. In this case, they monitor a queue of some kind, and when a message enters the queue, the WebJob performs some action on it. A WebJob can also be configured as a singleton or set up to run on all the instances of the application.

A container allows you to add all the dependencies of an application into a single package. This includes the application/custom code, its binary dependencies, and the operating system on which it runs. This allows the application to be portable across many platforms as all the dependencies are in a package that is fully functional on its own.

Azure App Service Web App for Containers supports Windows- and Linux-based containerized applications. When using the noncontainerized web app, the application runs within a sandbox, which imposes certain limitations. For example, writing the registry, accessing the event log, working with COM, and changing the order of the default cipher suite as mentioned before are not supported in the Azure Web Apps sandbox. You can, however, do all the previously mentioned activities in an Azure App Service Web App for Containers instance. Azure App Service Web App for Containers is even more flexible than ASE in many cases. As already stated, as a designer and an Azure architect, you must know the requirements of the application and the capabilities of the product for which you are planning your migration. It is possible to build your own container in Visual Studio or use an existing one hosted on Docker Hub, the Azure Container Registry, or GitHub.

Azure Mobile Apps was at one time called Zumo and is similar to Azure Web Apps but is optimized for a mobile scenario. Simple integration with a data source, easy integration with multiple authentication providers, and the ability to send push notifications to mobile devices are some of the specific features of an app on Azure Mobile Apps. As mobile apps typically operate on multiple operating systems such as iOS, Android, and Windows, there is a cross-platform SDK specifically created to work in this context. Autoscaling, monitoring, load balancing, and other basic features are the same between Azure Web Apps and Azure Mobile Apps.

Finally, let’s take a look at Azure API Apps. Don’t confuse Azure API Apps with API Management discussed later in this book. Azure API Apps is the quickest way to deploy and create RESTful APIs. While the apps require no infrastructure management, Azure API Apps has built-in support for Swagger. Azure Web Apps instances would typically have some kind of a GUI or an interface for an individual to interact in some way with the application. API Apps instances do not have a GUI or page to interact with; instead, they are endpoints that usually exchange and process JSON documents.

The following are some related products, features, and concepts to know in the Azure App Service context:

- Cloning
- Application Insights
- Backups
- Custom domains
- Managed identity
- SSL/TLS, App Service certificates
- Scale-up versus scale-out
- KUDU/SCM
- Cross-origin resource sharing (CORS)

Azure App Service is one of the main points of entry onto the Azure platform. This product can be configured to utilize and interface with almost all other Azure products and features. It is a cost-effective product, but keep in mind the sandbox in which the application must operate in; see the full list of products here:

```
github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox
```

within a single function/method of a typical nonserverless application. The point is, do not overload a single Azure Function with a massive workload. Although it will scale, it may not work as expected. Instead, break any large workload into smaller work sets and implement them using the Durable Functions model.

## API Management

Azure API Management (APIM) is a virtual gateway that is placed between consumers and actual APIs. You can think of APIM as a proxy or gateway in front of other APIs that reroutes the request to the proper API based on the parameters and signature of the URL. Why would this product be implemented? APIM provides the following benefits:

- Can expose separate APIs to external and internal consumers
- Hides the actual landscape from the consumer, which reduces complexity and confusion
- Controls usage and limits consumption
- Includes monitoring capabilities and error detection
- Connects to any and multiple APIs on the internet and exposes them via a single endpoint
- Allows group-based access control to APIs

The following are the scenarios in which APIM will be most beneficial:

- Integration
- Hybrid solutions
- Migration

From an integration perspective, assume that two companies have merged; both companies have internal and external APIs that are fundamental to the success of their company. Assume also that each of those APIs exposes a different endpoint, so managing, documenting, and knowing which to use and where to access it becomes possibly more complex than the API itself. What APIM can do is to expose a single endpoint where all APIs are accessible and manageable. This greatly simplifies the consumption, management, and maintenance of all APIs.

Hybrid solutions are when a company has created a product that depends on components hosted on-premise and in the cloud. Some customers want or need to keep some content or processes in-house for compliance or legal reasons. APIM can be used as a gateway between both of these on-premise and cloud-hosted APIs. APIM can be configured to run within a virtual network; then the virtual network is configured using a VPN or ExpressRoute to access on-premise resources. As mentioned already, APIM has solid authentication capabilities that are relatively simple to implement, directly in the portal.

Finally, imagine a scenario where a company has an API app running on an Azure App Service plan. This API would have a unique endpoint for all consumers to access so that its use case can be accessed. A change to the endpoint would require all consumers to update
