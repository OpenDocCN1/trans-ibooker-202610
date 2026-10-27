
- Reduced deployment failure / rollback. Since we have isolated the service boundaries, the scope of our deployments are also reduced, combined with automated testing and deployment of the application our success rate increases significantly.

There are two approaches you can take when choosing to refactor your application into a microservices architecture. (1) Spin up a team focused on rewriting the entire application using a greenfield approach. This approach will be less disruptive to your production application. However, it will add more risk to the overall project as you need the entire application to be re-written before you can realize the benefits. (2) apply the strangler pattern ([https://martinfowler.com/bliki/StranglerFigApplication.html](https://martinfowler.com/bliki/StranglerFigApplication.html)) to slowly iterate away the design of the legacy application and refactor it into microservices.

We recommend using the strangler pattern as it reduces the risk in adopting the new design patterns and overall refactoring of the application into a microservices architecture. This may add a little more work to the overall project since we need to extract the logic and plumb it back into the existing application. It allows us to incrementally improve our application architecture, providing more and more value each time we create a new production release. This reduces the amount of risk and starts to provide value more quickly to the business.

Many teams struggle with the simple question, where do I start? The answer will be different for each application. However, the pattern is generally the same, look for features that support the application and are easy to decouple. For example, a centralized authentication and authorization service is usually a good choice.

Consider leveraging the framework Google recommends ([https://cloud.google.com/architecture/migrating-a-monolithic-app-to-microservices-gke?hl=en#which_features_should_you_migrate_first](https://cloud.google.com/architecture/migrating-a-monolithic-app-to-microservices-gke?hl=en#which_features_should_you_migrate_first)) for evaluating which feature to move first. A summary of the framework has been provided below.

- Business process
- Design and development
- Operations
- People and teams

### Business process

You should evaluate and consider the impact moving the feature will have on the business users. There are a lot of new concepts the developers and operations teams need to learn to successfully modernize an application. Early in your modernization process you should avoid moving business critical systems or features.

### Design and development

Next you should evaluate the complexity of the feature, its dependencies and the amount of refactoring required to move it. Consider the following factors:

- Data usage, number of schemas (isolated or shared) and size
- Dependencies on other features
- Other features that depend on this feature
- Connectivity requirements
- Design elements that may create complexity in refactoring or have no clear solution

### Operations

While evaluating which features to move first you should evaluate the following:

- Service Level Agreements with the business
- Maintenance windows and tolerance for downtime

Consider focusing on features that are more lenient on downtime and don’t have critical business SLAs requiring High Availability.

![](img/fb3bebce7b5f593bca1a7f8a1371be06_740_0.png)

## 19.3 Benefits of Anthos for Microservices

The goal of a microservice architecture is to decouple the business service logic into individual self contained services to provide acceleration of development and optimization of performance. The drawback is that you are creating complexity in the configuration, deployment and maintenance operations of your application. Anthos solves the most common problems associated with supporting these types of applications.

- **Consistency:** a consistent runtime environment across multiple public cloud and on-premise data centers;
- **Automation:** a key to success with microservices is your ability to automate the deployment of your entire application. Anthos provides configuration as code to enable this capability. You can apply configuration, security policies, and even deploy your application components using ACM.
- **Dependency Management & Security:** increased complexity in decoupled services are more easily manageable using a service mesh. Instead of embedding physical service to service dependencies in our applications, a popular pattern in microservice based solutions is to embed logical references that are resolved by the service mesh. ASM provides this capability as well as the ability to inject security policy, enforce mutual TLS (mTLS) and bridge traditional VMs into your application as though they were other Kubernetes based services.
- **Observability:** understanding your application’s performance is more difficult once you have broken it into microservices. Instead of monitoring one Java process with a sophisticated tool you will need to monitor lots of services for performance and understand when one is degraded or broken. Anthos Service Mesh (ASM) provides the ability to visualize and inspect your application performance as well as set Service Level Objectives (SLOs) so that you can be alerted or have automated processes remediate service performance.

## 19.4 Real World Examples

There are lots of real world examples where we need to run compute workloads close to the user (aka running on the edge). These are great use cases for Anthos which provides our ability to apply configuration and security policies as code. A few easy to identify are those use cases that apply to retail or manufacturing.

Imagine that you are a large cruise line, you have developed a new user experience leveraging many of the concepts we have described in this book. Your team has built a new microservice based architecture with web and mobile application user experiences. This new digital experience is used to provide lots of shipboard capabilities to your customers, like activity planning, meal ordering, events, announcements, and concierge services. The problem you keep having is that there is no consistency in the fleet. You are running into problems keeping the hardware and software updated on each ship leading to an inconsistent user experience, outages and many more problems. The effort to update is a time consuming human effort to apply patches and upgrade software. So even though your development team took a modern approach and you are struggling to operate this complex solution. You need Anthos, while it won’t eliminate the process of updating hardware or core networking changes. It will handle everything else, tightening the gap between manual and automated processes and reducing your overhead to operate the solution. Anthos will provide you with the following capabilities to get a handle on your environments and avoid configuration and deployment issues:

- Anthos GKE, will provide you with a managed Kubernetes distribution that ensures you are running the same runtime platform on each environment.
- Anthos Service Mesh, will provide you with security and observability capabilities to ensure your container environment and workloads are secure and that your developers can easily find problems by using Service Level Objectives (SLO).
- Anthos Configuration Management (ACM), will provide you with a centralized policy as code approach to core cluster configuration, security policies and application deployment consistency.

Now imagine that your team has built the solution with Anthos at the center of operations. Your team has designed each cluster and documented your core cluster configuration and security policies as code leveraging Anthos Config Management. To bring a new shipboard data center online it’s as simple as racking & stacking the gear, getting the HW on the network, installing Anthos and then registering the cluster with the control plane and ACM. Then ACM will bootstrap the cluster with the right configuration and security policies.

## 19.5 Anti-Patterns to avoid

There are a number of anti-patterns we have discovered over the years helping teams modernize their applications into a microservice architecture.

- Big bang approaches are tempting: many teams decide to scrap what they have and start over re-writing the entire application all at once into a microservice architecture. In our experience this tends to lead to a waterfall approach, significantly longer efforts before the business can realize the benefits and overrun budgets. A better approach is to peel the onion, one layer at a time as we have mentioned before in the chapter
