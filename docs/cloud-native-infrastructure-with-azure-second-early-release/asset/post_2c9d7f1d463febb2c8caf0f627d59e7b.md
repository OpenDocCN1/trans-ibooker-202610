
## Inventory file

The inventory file holds the list of all the hosts Ansible is managing. The inventory usually holds IP addresses, hostnames, the SSH username, and keys to connect to the remote machine. The following is an example of the inventory:

```
mail.example.com

[webservers]
foo.example.com
bar.example.com

[dbservers]
one.example.com
two.example.com
three.example.com

[midtierrservers]
10.0.1.2
10.2.3.4
```

Since Ansible does not require a lot of overhead and is written in YAML, it serves as a great post-configuration tool. We will be using Ansible in this book at times to provision parts of the services.

Before we wrap up this chapter, let’s take a quick look at Azure DevOps, which helps us combine all of these tools in an automated fashion to create a CI/CD pipeline.

## Azure DevOps and Infrastructure as Code

Azure offers *Azure DevOps*, which provides an end-to-end DevOps toolchain for developing and deploying applications and is primarily a hosted service for deploying CI/CD pipelines. You can use Terraform, Ansible, and Packer together as well, and build multistaged pipelines for your projects. Azure DevOps has a bunch of services, including *Azure Pipeline*, which is primarily used for building, testing, and deploying the CI/CD pipelines. We will not take a deep dive into Azure DevOps since it’s beyond the scope of this book, but you can read more about setting up Azure DevOps in the official documentation.⁹

## Summary

In this chapter, we introduced the steppingstones for building a modern cloud native environment using Azure. You created an Azure account, which will come in handy in upcoming chapters as you gain a deeper understanding of cloud native technologies. You were also introduced to Terraform, Packer, and Ansible: three helpful cloud native orchestrators that will assist you in deploying and managing your cloud infrastructure. Terraform is a cloud-agnostic tool for developing infrastructure as code from the ground up, Packer is used to create machine images for development of immutable artifacts, and Ansible is a configuration management tool.

With this basic understanding of Azure and related technologies under your belt, we can move on to the next chapter, where we will talk about containers, the container registry, and containerizing your application.

3 MFA adds a layer of protection to the sign-in process. When accessing accounts or apps, users provide additional identity verification, such as scanning a fingerprint or entering a code they receive on their phone.

4 Azure Cloud Shell comes with the latest version of Cloud Shell. For information on how to get started with Terraform on Cloud Shell, visit https://docs.microsoft.com/en-us/azure/developer/terraform/get-started-cloud-shell.

5 An Azure service principal is an identity created for use with applications, hosted services, and automated tools to access Azure resources. This access is restricted by the roles assigned to the service principal, giving you control over which resources can be accessed and at which level.

6 Azure offers JSON (JavaScript Object Notation) by default for writing ARM templates. Azure also offers a domain-specific language called Bicep that also uses declarative syntax to deploy Azure resources. You can use Bicep instead of JSON since it reduces complexity and improves the overall development/management experience.

7 Prebaked is a term used to refer to preinstalling software onto a machine image which can be used later to spin more instances or containers.

8 Packer is available on Azure Cloud Shell by default.

9 https://azure.microsoft.com/en-in/services/devops

## Chapter 3. Containerizing Your Application: More Than Boxes

## A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 3rd chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at rfernando@oreilly.com.

Over the past several years, containers have grown in popularity. Not only do they offer low overhead, high security, and high portability, they also follow best-practice cloud principles such as immutability, ephemerality, and autoscaling.

In this chapter, we’ll explain what containers are and discuss the popular containerization platforms. In addition, we’ll look past the general industry hype and explain why you might want to use containers. We’ll also highlight some of their benefits, especially when it comes to the cloud.

## Why Containers?

Containers run on top of the host operating system and usually are orchestrated using software like Docker (as we’ll discuss later in this chapter) or Kubernetes (see Figure 3-1). One of the most important benefits

of containers is that they provide virtual-machine-like isolation, but without the overhead of running an instance of the host operating system for each container. At scale, this can save a significant amount of system resources.

![](img/8d47d1b5dbca55e784ae602145a894c7_61_0.png)

Figure 3-1. The container operating paradigm

## Isolation

Containers provide isolation via a number of methods in a flexible manner that is configured by the administrator. Containers can be isolated in the following ways:

## System resources

Isolation is achieved via the CPU, memory, and disk cgroups.

## Namespaces

Isolation is achieved by allowing separate namespaces for each container.

## POSIX limits

Isolation with this method allows you to set rlimits on your container.

These mechanisms allow you to safely run numerous workloads together without worrying about resource contention or one system disturbing another. If you have a container orchestrator that is workload aware, you can efficiently utilize all of the resources on a host machine without performance degradation (known as *bin packing*). Done well at scale, this can create significant cost savings as you increase your infrastructure efficiency.

## Security

Building off the container isolation features, the OCI container runtime specification (discussed later in this chapter) provides a number of runtime-security-related features that ensure that if an application inside a container is compromised, the risk of lateral movement throughout your network is significantly reduced. Some of these features include:

- *Linux capabilities*

These reduce the container’s access to kernel APIs.

- *User/group attributes*

These set the user and group attributes of the container, which can then limit access to filesystem resources.

- *Devices*

These control which devices a container has access to.

- *SELinux labels*

Like other components of a Linux system (files, processes, pipes), SELinux labels can be applied to containers.

- *Sysctl*

This adds (limited) additional controls to the container.

These layers of runtime security features provide a deterministic, managed way to deploy applications with significantly diminished risk to the host infrastructure and neighboring applications. Within the container ecosystem, there is also a suite of artifact security paradigms which we will cover later in this chapter.

Note that if you want to apply system policy via an orchestration mechanism (Puppet/Chef, etc.), it is much more work to implement it on a per-application basis than it would be to just use the features the container provides.

## Packaging and Deployment

As we’ll discuss later in this book, the standardization of container images and the portability of those images is extremely useful. It is relatively straightforward to take a container that’s running on your desktop and deploy it to production without significant operational overhead or having to perform extra configuration and administration steps. Docker’s mantra is “Build, ship, and run,” which highlights the desktop-to-production mentality that containerization brings to the software development lifecycle.

Furthermore, given the OCI specification of images, as we’ll discuss later in this chapter, you can feel confident that the deployment system will succeed without issue as long as your container registry is available.

## Basic Container Primitives

You may be surprised to know that containers are not a first-class concept within Linux. In fact, there was no formal definition of a container until the creation of the OCI specification in 2017 (we’ll talk more about specifications later in this chapter). A basic container is generally made up of the following primitives:

- Cgroups
- Namespaces
