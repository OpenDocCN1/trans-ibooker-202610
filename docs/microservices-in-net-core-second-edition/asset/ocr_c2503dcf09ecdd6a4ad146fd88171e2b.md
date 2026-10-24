across different environments, we can be reasonably confident that it will work the same across environments.

Looking at the Dockerfile, we notice that there is nothing about it that makes it specific to the shopping cart, except the names of the .csproj file and the .dll file. So not only have we built a shopping cart container that we can run in a variety of environments, we have learned how to do the same for all the other .NET-based microservices we will build.

The next thing we need to do is to run the shopping cart container in Kubernetes.

# 3.3 Running the shopping cart container in Kubernetes

Now that the shopping cart is in a container, it is ready to run in Kubernetes. We will create a Kubernetes manifest file and use it to run the shopping cart on Kubernetes—first on localhost and then on Azure. Running the shopping cart container in Kubernetes looks like figure 3.3, which is almost exactly like the diagram at the beginning of the chapter except Kubernetes is running on localhost instead of on Azure. This is no accident, since we are going run the shopping cart in the same way in localhost as we will on Azure.

![](bbox=[0.25, 0.44, 0.7, 0.78])

Figure 3.3
Running the shopping cart on Kubernetes on localhost is almost the same as on Azure.

# 3.3.1 Setting up Kubernetes localhost

How to set up Kubernetes on your development machine depends on the operating system you are using. If you are on Windows or Mac, I recommend using Docker Desktop, which comes with the option to enable Kubernetes. If you are on Linux, there are numerous options; one easy option is MicroK8S.