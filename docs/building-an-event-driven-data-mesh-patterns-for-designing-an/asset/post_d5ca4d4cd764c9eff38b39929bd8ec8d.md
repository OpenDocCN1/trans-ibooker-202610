
all and focus on deriving value from the data itself, building the data products, and learning to use the cloud platform. You’ll get feedback on what works and what doesn’t far sooner, and you can switch technology selections if you find your initial choices unsuitable, without having invested heavily in infrastructure and operations. Your main goal should be to _try things out_ to find what works before overinvesting. You can always optimize later; at this point, you’re trying to find what works and what doesn’t. Cloud computing makes this a far easier task.

That’s it for Level 2. Let’s take a look at what we covered and how it’s all supposed to work together.

## Level 2 Wrap-Up: How Does It Work?

The EP seeks to address the deficits of the MVP pertaining to service and user identity. It also provides the means for improved data product management and discovery, based on a full-featured data catalog.

The centralized data product management service and UI form the basis of the user-facing controls. Users can discover published data products, view metadata, and decide which data products they need for their own service’s use cases. The management service stitches together the event broker, metadata catalog, schema registry, and subplatforms into a self-service experience. The self-service UI exemplifies the consumer-focused aspect of building a data mesh: make it easy for users to find and use data products.

However, the code and compute power that create data products remain outside the scope of what we’ve covered so far. We’ve alluded to using services like GitHub, Confluent Cloud, and Kubernetes to store code, host event streams, and provide general-purpose compute, respectively. But we haven’t integrated these into our full-fledged self-service platform yet.

The self-service platform is also far from fully automated. Humans remain involved in the day-to-day servicing of platform requests. Administrators manually manage roles, accounts, and permissions for both services and humans.

Getting to Level 3 is fairly difficult, and like all things data mesh, should only be attempted if you’re finding that the pain points of Level 2 exceed the investment costs of the self-service platform. In the next section, we’ll investigate the features of the fully mature platform.

## Level 3: The Mature Platform

The mature platform (MP) is the culmination of stitching together identities, services, processes, and data products into a cohesive set of streamlined operations. It leans heavily on implementing identities, access controls, and workflows spanning multiple services, platforms, and technologies. The MP focuses on reducing the manual steps and repetitive overhead encountered in the earlier self-service platforms and on hardening and streamlining a select set of data mesh operations for daily operations.

For example, creating a data product consumer application using Kafka Streams may be a primary use case for your business. So you streamline the creation of the Git repo, the Docker container and repository storage, the Kubernetes task, the testing, compilation, and deployment pipeline, as well as the monitoring, permission assignment, and service ownership rights—all in a single button from the self-service UI. If this sounds like a lot of work, well... it is! But as your data mesh needs grow, so too will your need to streamline its use by investing in new functionality.

The MP is a target more than an actual destination. If you find that your simpler platform with less automation and streamlining works fine, then great! Keep on using it until you find it’s not meeting your needs. Stitching together multiple services, cross-platform identity management, teams, people, and systems into a centralized control panel is pretty tough. Remember, there are entire cloud service providers who have spent a ton of time trying to get this right. If you find it challenging, there’s a good reason for it.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_130_0.png)

> You’re most likely going to need to use an Infrastructure as Code solution like Terraform to help manage the infrastructure components of your data mesh. Terraform is one of the leading infrastructure management solutions and can integrate “stitching” operations between many cloud providers, SaaS solutions, and platforms.

Efficiency is your largest gain from implementing the features of the mature platform. It’ll be far easier for your average user to create, publish, manage, and consume data products. You will encounter far fewer incidents and outages, data quality issues, and end user-affecting data discrepancies. You can expect to see a substantial reduction in the overhead for finding and using data, as discussed earlier in “Bad Data: The Costs of Inaction” on page 17.

The components and features of the MP include:

- Authentication, identification, and access management
- Integration with existing application delivery processes
- Programmatic data product management API
- Monitoring and alerting
- Multiregion and multicloud data products

First, let's take a look at standardizing identity and access management across the self-service platform.

## Authentication, Identification, and Access Management

Authentication and identification and access management (IAM) is an extensive area in its own right, and we won't be able to do it full justice in the confines of this chapter. Much of the work regarding authentication and IAM is related to stitching together the various identities, roles, and permissions across each of the major infrastructural components in your platform. For example, Kubernetes, Apache Kafka, and GitHub/GitLab each offer their own identity management system, but only within the confines of their own service. Given that our data products consist of code, data, and infrastructure, we're going to need to find a way (like using Terraform) to bridge the gaps and stitch together a unified identity experience.

But what is authentication, and what is authorization? Think about when you check into a hotel. The front desk staff _authenticates_ you by asking for your ID or your booking code to make sure you are who you say you are. Next, they _authorize_ you to access your room by giving you a key card—the card works on only that room and will expire when it's time for you to check out. Both authorization and authentication are essential for stitching together a data mesh, and these requirements are best fulfilled using standard frameworks like OAuth2 and the accompanying OpenID Connect (OIDC).

Fortunately, the roots of this problem have largely been solved over time. There are best practices that we can follow and existing technologies that provide a good place to start building your self-service platform.

OAuth2 is one of the leading industry protocols for authorization. As described in the original Internet Engineering Task Force (IETF) specification, “The OAuth 2.0 authorization framework enables a third-party application to obtain limited access to an HTTP service, either on behalf of a resource owner by orchestrating an approval interaction between the resource owner and the HTTP service, or by allowing the third-party application to obtain access on its own behalf.”

Kubernetes, GitHub, GitLab, and Apache Kafka each integrate and support OAuth2, so you'll be able to go quite a long way in unifying your authentication and authorization controls. OAuth2 is a fairly common standard, so you'll most likely find it easy to extend into the other cloud services and platforms that you're already using.

An OAuth2 service provides authorization through the use of access tokens. When one application wants to talk to another application, it first makes a request to the OAuth2 server for an access token. The requesting application identifies itself to the OAuth2 server and, depending on business rules and permissions, may obtain a bearer token. The requesting application can then take that bearer token and present it to the original application it initially wanted to connect to.

OIDC works hand-in-hand with OAuth2, making it possible to enable data sharing between applications without directly sharing user and service credentials. OIDC extends OAuth2 by focusing primarily on authentication rather than authorization. Once an account is authenticated using OIDC, it then uses OAuth2 specifications for authorization. OAuth2 helps alleviate the major issues inherent in dealing with multiple different services and frameworks in your data mesh and allows you to stitch together a cohesive experience.

There are several major benefits of uniting under OAuth2. Your self-service users will be able to:

- Declare and manage user and service accounts in a single place
- Rely on the OAuth2 standards and tools to integrate with your cloud service provider platforms and applications
- Manage most permissions and access controls from a single location
- Enforce authentication and authorization as a mandatory step for building and using data products

There are other technologies you could use to achieve similar ends; this is just one option. For example, Security Assertion Markup Language (SAML) 1.0 was released in 2002 and remains a commonly used alternative. In any case, the end goal remains the same: unify and streamline identity management and access controls among people, systems, and data products.

Next, let's take a look at how we want to be able to deliver and deploy data products.

## Integration with Existing Application Delivery Processes

There's a pretty good chance that you already have some sort of existing application development and deployment pipeline. A useful and sustainable self-service data platform should reuse as much of this existing pipeline as possible. A data product is quite similar to any other application, is particularly similar to microservices, and especially similar to event-driven microservices. In fact, if you were to look at them side by side, you would probably have a hard time defining the difference between an event-driven data product and an event-driven microservice: both use code, compute resources, and memory, and bundle up their results as data served out into event streams.

Because you're likely to be making a number of data products, you'll be running into the same sort of overhead that one runs into when using microservices—the micro-service tax. Each new data product will need a Git repository and a build pipeline,
