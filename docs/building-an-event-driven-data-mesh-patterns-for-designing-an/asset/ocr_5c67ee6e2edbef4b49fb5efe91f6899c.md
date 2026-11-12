plus a way to containerize the compiled application, deploy it to compute resources (such as via Kubernetes), scale it, monitor it, and update it.

However you have chosen to build your existing application delivery process, you'll need to evaluate how to leverage it to deploy your data products. If you have a monolithic tech stack with only a handful of custom-built and deployed services, you'll likely find this to be quite challenging. However, if you're already running multiple services with a relatively well-formed deployment and management process, you're likely to find this much easier.

In “Stream Processing for Building Data Products” on page 114 we looked at some options for building and using data products using stream-processing frameworks, and, in particular, we looked at using cloud services instead of hosting your own solutions. If you chose to follow the cloud route, you'll likely be able to leverage their recommended best practices for delivering data product applications into production. You'll find that many of the cloud providers make it easy to manage and deploy the “code container” of their choice, be it Docker images, Notebooks, or persistent queries. Regardless of where you may be, my advice to you remains the same: try to find a cloud service provider that can offer you an easy way to build, test, store, deploy, and run your data product code.

# Programmatic Data Product Management API

The data product management UI is expanded to provide a programmatic API that integrates with your identity authentication and authorization service. The goal of this feature is to provide an API for automated management of data products, including publishing, updating, deprecating, and deleting. This API will likely be just an expansion of the API powering the self-service UI, extended to provide API access to whatever tools and services may choose to integrate with it. Outside of direct user-facing use cases, other candidates for API usage include deployment pipelines, monitoring systems, and auditing use cases.

Let’s avoid the painstaking details of an in-depth API spec and focus on the areas that we need to support with this API:

+   **Identity management**
+   Streamlining the creation and management of both human user and service account identities is essential for coordinating the assignment of resources and permissions for all candidate data products.

+   **Resource creation**
+   All the resources required for creating a candidate data product can be created in the form of an application. Resources include a service identity, a code repo, build and deployment pipelines, Kubernetes resources, Kafka topics, schema reg-