Kubernetes, and those are the ones running global reach workloads. Anyway, here’s some considerations:

+   - Kubernetes doesn’t know how to handle our app. It can restart it if fails, but cannot understand why that is happening, so we need to ensure we have full control of our microservices-based architecture and be able to debug each container. This is particularly important in case of a large scale deployment.
+   
+   - Kubernetes doesn’t provide any middleware or application-level services. Granular discovery services need to be addressed by interacting with Kubernetes API or relying on some service on top of Kubernetes, such as a Service Mesh framework. There is no ecosystem for developers out of the box.
+   
+   - Kubernetes doesn’t build our app. You are responsible for providing your app compiled and packaged as a container image or relying on additional components on top of Kubernetes.

With that in mind, let’s start digging into a Kubernetes journey for developers, to make our first steps to bring our app into the next cloud-native production environment.

# Infrastructure as a Code

Kubernetes provides a set of APIs to manage the desired state of our app as well as the whole platform. Each component in Kubernetes has an API representation that can be consumed. Kubernetes offers a declarative deployment pattern that allows you to use a way to automate the execution of upgrade and rollback processes for a group of Pods². The declarative approach is granular and it is used also to extend Kubernetes APIs with the concept of *Custom Resources*.