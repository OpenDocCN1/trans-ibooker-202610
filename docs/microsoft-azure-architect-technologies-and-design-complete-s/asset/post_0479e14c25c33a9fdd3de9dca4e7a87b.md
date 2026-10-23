
In the previous section, you read about a similar capability with Application Gateway. Application Gateway does provide many capabilities and features and is a valid choice for this similar scenario. The reason for having multiple products for similar scenarios is just that the scenarios are only similar; some designs or some applications running on Azure need additional or different features, and instead of putting all features into one product, many products get created. One must know the requirements of the application moving to Azure and which Azure product provides the best set of tools to meet those requirements. Azure Load Balancer provides some unique features that are discussed in the remainder of this section.

- Public load balancer
- Internal load balancer
- Health probes
- Outbound connections

Azure Load Balancer has two SKUs available, Standard and Basic. For any new implementation, it is recommended that you use the Standard SKU. It is the most secure, robust, and scalable of the two. Availability Zones, an internal load balancer, the configuration of outbound security rules, and an SLA are only available with the Standard SKU.

## Public Load Balancer

During the creation of an Azure Load Balancer instance, one must choose whether it will be internal or public. Choosing a public load balancer will generate a public IP address for clients to use for connecting to the servers configured in the backend pool. Figure 3.68 illustrates the flow of a request from a client to servers in a pool of Azure VMs.

![](img/95043b3df926bcdadadb082b49262037_253_0.png)

Both internal and public load balancers by default will route traffic equally to the Azure VMs configured into the backend pool. This is often referred to as round-robin. Using a five-tuple hash that consists of source IP, source port, destination IP, destination port, and the protocol, the load balancer routes the request to the same Azure VM for the entire transmission session. Any change in the hash values will result in the request being sent to a different server. This may be an issue if there is a need to maintain a session between the client and server. If this is the case, then Azure Load Balancer does support this using the feature called source IP affinity mode. This mode uses a hashed version of the source IP, destination IP, and/or the protocol to route requests to the same Azure VM for the life of the session.

## Internal Load Balancer

An internal load balancer (ILB) will not have a public IP address and is instead configured into an existing virtual network. As shown in Figure 3.69, traffic is load balanced across a group of Azure VMs, and the connectivity from those VMs to other servers in the VNet is managed using an ILB.

FIGURE 3.69 Internal load balancer flow

![](img/95043b3df926bcdadadb082b49262037_254_0.png)

The ILB could be managing access to a group of database servers, REST APIs, or some other kind of resource needing this added level of security and resiliency.

## Health Probes

Health probing is a feature that monitors the status of Azure VMs configured in the backend pool. There are three probe types, TCP, HTTP, and HTTPS. (HTTPS is available only in Standard SKU.) The TCP probe will start a three-way TCP handshake on the configured

Most of those features are understood simply by the name, and many have already been discussed. A few of them may not be so clear and need a little more explanation.

## Multisite Hosting

Similar to the URL-based routing, URL redirection, and the URL-based redirection configured with Application Gateway in Exercise 3.13, multisite hosting is a feature that routes requests to different backend pools based on the requested URL. The primary difference with this feature is that the configuration of the domain names linked to Front Door is performed on the blade for the product being configured. Multiple domain names and subdomains can be hosted and configured directly on the Front Door portal blade. Requests to those domains will route directly to Front Door.

## Custom Domains and Certificate Management

To support the multisite hosting feature, Front Door needs to support custom domains. In addition to custom domains, SSL certificates are also supported for communicating over HTTPS. The configuration of those two capabilities is built into the portal and also available using PowerShell.

## Application Layer Security

Application layer security is synonymous with a web application firewall. The WAF contained within Front Door is primarily focused on attacks that happen at layer 7 such as SQL injection, cross-site scripting (XSS), and session hijacking. These kinds of vulnerabilities generally happen using the HTTP and HTTPS protocols. The concepts are the same with regard to Azure Firewall, Application Gateway (WAF), and Azure Front Door (WAF). A policy that decides whether to prevent or detect an attempted exploit is created. Review Exercise 3.10 if this is not ringing any bells. Once the policy has been created, a group of rules (commonly rules that would prevent the known vulnerabilities listed in the OWASP top 10 list) is applied. When you apply those configurations to a network device, you can sleep a little bit better knowing your application is as safe as possible.

One last capability that is available has to do with backend pools. Up to now the only resource types that could be configured into a backend pool have been Azure VMs contained within a VNet. Azure Front Door also supports products such as App Service, Azure Functions, Azure Storage, and some others. This feature allows much more control and provides more configuration opportunities. Placing a WAF in front of an Azure Functions instance or directing a specific host name to an Azure Storage container are options that come to mind.

## Azure Content Delivery Network

A content delivery network (CDN) is used to get content as close as possible to the clients who need access to it. Azure has data centers all over the world. These result in better performance for those closest to the regional locations. However, there can be some

significant improvement for customers or employees by getting the content hosted physically closer to them. In Exercise 3.1, you used Network Watcher to measure latency between two locations. Now may be a good time to go review it to see the possible latencies your customer could expect and perhaps how much faster the responsiveness of the site can be with a CDN.

To learn more about Azure Content Delivery Network, perform the following exercise. Chapter 4, “Compute,” covers creating the App Service web app required to complete this exercise. If you are not familiar with that process, consider skipping Exercise 3.15. When you have completed Chapter 4, come back to this exercise.

## EXERCISE 3.15

## Configuring a CDN for Azure App Service

1. Log in to the Azure Portal at portal.azure.com.
2. Create an Azure App Service web app (for example, csharpguitar). See Chapter 4 for more details or follow the online instructions at docs.microsoft.com/en-us/azure/app-service.
3. Click the Networking link in the Navigation blade and then click Configure Azure CDN for your app.
4. In the New Endpoint section, create a new endpoint (for example, CSHARPGUITAR-CDN-ENDPOINT). Select the pricing tier (for example, Standard Microsoft) and CDN endpoint name (for example, CSHARPGUITAR) and then click the Create button.

That was easy because all the magic happened behind the scenes. Configuring a custom domain (for example, www.csharpguitar.com) is most optimal in this scenario. During that process you would bind the custom domain to the azureedge.net domain so that requests made to the custom domain are responded to from the CDN. This works in the background based on the 120+ point-of-presence (POP) locations around the world. These POPs have edge servers that are cached with the content you want to deliver to those requesting it. A request to the domain of the site is routed to the nearest edge server for response generation. It is expected to be faster because the content is closer. It really can be significant if the content is large such as videos, audio files, or any content that consists of multiple megabytes.

A few additional features provided by an Azure CDN follow:

- CDN caching rules
- Geofiltering
- File compression
- Dynamic site acceleration
