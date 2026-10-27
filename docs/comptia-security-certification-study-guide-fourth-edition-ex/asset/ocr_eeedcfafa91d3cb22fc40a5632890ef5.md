# Containers

As mentioned earlier in the chapter, a container is a feature that allows you to run an application in an isolated environment that has its own resources assigned to it, much like a virtual machine. A container differs from a VM in the sense that a container is more lightweight because it shares the OS kernel of the host system. This is why a container performs better than a VM while still providing the isolation features of a VM. Docker is an example of software that provides containers and is a feature of Windows 10.

# Microservices/API

A microservice is a piece of functionality, or component, that is programmed into an application programming interface (API) that can then be called by other applications. The benefit of the microservice is that each component is a complete unit of work and can be released and updated separately from other components in the API.

An application programming interface is a collection of functions a developer creates that allows other developers to call upon this functionality. For example, let’s say we run a company that creates a firewall product. As part of the firewall device, we have a web interface that has all the common firewall configuration options to allow administrators to configure it. As the creator of the product, we could also create an API with a list of functions that are the different types of configuration changes that can be made to the device (see Figure 16-3). The benefit of creating the API is that it allows other developers to create their own web admin pages if they do not like the web page we created. When a developer creates their web page, they simply add links or buttons to it that make calls to the API. Note that today we call these “web APIs” because they occur over HTTP traffic.

# Infrastructure as Code

One of the big benefits of cloud-based environments and infrastructure components integrated within the cloud is that we can leverage technologies such as software-defined networking and software-defined visibility:

+   Software-defined networking (SDN) SDN is a way to manage network device configuration from a central point. SDN uses a controller application and APIs to centrally deploy configuration changes to network devices.

+   Software-defined visibility (SDV) SDV is a method to monitor the entire network environment with the capability to respond to security incidents in real time.

![](bbox=[0.07, 0.82, 0.85, 0.925])