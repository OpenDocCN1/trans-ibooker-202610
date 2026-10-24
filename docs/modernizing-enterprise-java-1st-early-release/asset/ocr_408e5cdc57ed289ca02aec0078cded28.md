[Ansible]( ), [Helm]( ), and [Kubernetes Operators]( ) help you. We talk more about automation in chapter four, and you’ll use an Operator in chapter seven.

# What “Cloud Native” means

You’ve probably heard of the *cloud-native* approach for developing applications and services, and even more so since the Cloud Native Computing Foundation (CNCF) was founded in 2015 and released Kubernetes v1. Bill Wilder first used the term cloud-native in his book, [Cloud Architecture Patterns]( ). According to Wilder: A cloud-native application is architected to take full advantage of cloud platforms by using cloud platform services and scaling automatically. Wilder wrote his book during a period of growing interest in developing and deploying cloud-native applications. Developers had various public and private platforms to choose from, including Amazon AWS, Google Cloud, Microsoft Azure, and many smaller cloud providers. But hybrid-cloud deployments were also becoming more prevalent around then, which presented challenges.

The CNCF defines cloud-native as:

> *Cloud-native technologies empower organizations to build and run scalable applications in modern, dynamic environments such as public, private, and hybrid clouds. Containers, service meshes, microservices, immutable infrastructure, and declarative APIs exemplify this approach.*

> *These techniques enable loosely coupled systems that are resilient, manageable, and observable. Combined with robust automation, they allow engineers to make high-impact changes frequently and predictably with minimal toil.*

—CNCF Cloud Native Definition v1.0,
https://github.com/cncf/toc/blob/master/DEFINITION.md[
github]]

Similar to cloud-native technologies are [The Twelve-Factor Apps]( ). The Twelve-Factor apps manifesto defines patterns for building applications that are delivered on the cloud. While these patterns overlap with Wilder’s cloud