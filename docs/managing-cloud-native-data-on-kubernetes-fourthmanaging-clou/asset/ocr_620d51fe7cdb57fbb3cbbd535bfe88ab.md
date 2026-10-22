Kubernetes which is evident in the incredible growth. This speaks to how well it solves the problem. According to [to the Kubernetes documentation](to the Kubernetes documentation):

> *Kubernetes is a portable, extensible, open-source platform for managing containerized workloads and services that facilitates both declarative configuration and automation. It has a large, rapidly growing ecosystem. Kubernetes services, support, and tools are widely available.*

Kubernetes was originally designed for stateless workloads, and that is what it has traditionally done best. Kubernetes has developed a reputation as a “platform for building platforms” in a cloud-native way. However, there’s a reasonable argument that a complete cloud-native solution has to take data into account. That’s the goal of this book: exploring how we make it possible to build cloud-native data solutions on Kubernetes. But first, let’s unpack what that term means.

# What is Cloud Native Data?

Let’s begin defining the aspects of cloud native data that can help us with a final definition. First, let’s start with the [definition](definition) of cloud native from the Cloud Native Computing Foundation (CNCF):

> *Cloud native technologies empower organizations to build and run scalable applications in modern, dynamic environments such as public, private, and hybrid clouds. Containers, service meshes, microservices, immutable infrastructure, and declarative APIs exemplify this approach.*

> *These techniques enable loosely coupled systems that are resilient, manageable, and observable. Combined with robust automation, they allow engineers to make high-impact changes frequently and predictably with minimal toil.*

Note that this definition describes a goal state, desirable characteristics, and examples of technologies that embody both. Based on this formal definition, we can synthesize the qualities that make a cloud native application differentiated from other types of deployments in terms of how it handles data. Let’s take a closer look at these qualities.