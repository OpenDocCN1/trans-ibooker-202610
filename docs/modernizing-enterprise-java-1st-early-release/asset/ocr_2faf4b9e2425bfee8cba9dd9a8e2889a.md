| Instruction | Description |
|---|---|
| FROM | Used to inherit from a base image. It can be for example a Linux distribution like `fedora,centos,rhel,ubuntu`. |
| ENV | Use environment variable for the container. These variables will be visible to the application and can be set at runtime. |
| RUN | Execute a command in the current layer, like installing a package or executing an application. |
| ADD | Copy files from your workstation to the container layer, like a JAR file or a configuration file. |
| EXPOSE | If your application is listening to some port, you can expose it to the container network so that Kubernetes can map it to a Pod and a Service. |
| CMD | The command you use to start your application, the final step of the container image building process where you have all your dependencies in the layers, and you can run your app safely. |

The process for creating your container from your Dockerfile is described also in Figure 4-3.

![](bbox=[0.15, 0.58, 0.88, 0.73])

Figure 4-3. Building Container Image

An example of a Dockerfile for the Inventory Quarkus Java microservice that we created in Chapter 2 is listed below, and you find it in this book’s GitHub repository.