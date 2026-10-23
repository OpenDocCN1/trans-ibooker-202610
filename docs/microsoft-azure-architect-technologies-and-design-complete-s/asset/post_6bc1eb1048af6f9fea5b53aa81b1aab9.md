
container is often referred to as the runtime. The following are a few benefits for running application code in containers:

- Containers are portable.
- Containers are lightweight.
- Containers are flexible.
- Containers are scalable.

From a portable perspective, containers allow a developer or release manager to have confidence that the code in the container will run anywhere, on a local development environment, on a corporate on-premise physical server, or in the cloud. It will run anywhere without making any configuration or coding changes because it has no system dependencies. A container is lightweight because it reuses the operating system of the host; there is no need for an operating system to be deployed along with the container. It is flexible because all kinds of programs—small, large, simple, or complex—can run in a container. They are scalable, which means the number of servers that can simultaneously run the container can be added to provide more compute power when usage increases.

When a container package is deployed to ACI, it receives a public-facing IP address and domain name with the extension *<region>.azurecontainer.io, where * is the name of the ACI container that must be unique for the given region. A region, as covered in detail in Chapter 3, is the geographical location that the ACI will be installed into. This is selected during the container’s creation. Regions are, for example, northeurope or southcentralus. Keep in mind that ACI is not yet available in all Azure regions; however, the deployment globally is ongoing and will reach worldwide scale in the short term.

ACI offers support for containers running on either Windows or Linux. The container concept has been mostly focused on the open source community and therefore Linux, so you will find the number of supported features for Linux images is greater than that currently for Windows. Table 4.1 lists the currently supported Windows base images on ACI.

## TABLE 4.1 ACI-Supported Windows Images

| Server Version | Edition | Version |
|---|---|---|
| Windows Server 2016 | Nano Server | 10.0.14393.* |
| Windows Server 2016 | Server Core | 10.0.14393.* |
| Windows Server 2019 | Nano Server | 10.0.17763.* |
| Windows Server 2019 | Server Core | 10.0.17763.* |
| Windows Server 2019 | Windows | 10.0.17763.* |

The thing about Linux is that there are a lot of versions and editions. And when I write “a lot,” I mean a lot. In reality, there can be an infinite number of versions and editions because the Linux operating system is open source, and if I were so inclined, I could create my own version and edition of Linux, and it would run on the ACI platform. Therefore, there is no table that defines which Linux images will run on ACI. Be assured, however, that mainstream versions of Linux will deploy and run as expected on the Azure Container Instances platform.

## OS Virtualization, Containers, and Images

If you are like me, meaning most of your career has been in the Microsoft world of Windows and the .NET Framework, then the concept of images and containers may be a bit of a mystery. This is common because like I wrote earlier, this concept was mostly confined to the open source community, which until recently Microsoft was not actively engaged in. Although there are numerous products that provide OS-level virtualization, the most common one is Docker. The Docker software is less than a decade old, which may seem like a long time when considering “cloud speed” but in reality is not. Docker became a publicly available open source product in 2013 and only became supported in Windows Server 2016, which was shipped that same year. Based on that, I am confident that you will agree containers are a relatively new concept, especially from a Windows perspective. Be assured, however, that this area is picking up steam and will become a must-know skill set, especially for an Azure Solutions Architect Expert.

Let’s look at what OS-level virtualization is in a bit more detail. Take a look at Figure 4.7, which compares a virtual machine with a container. We have not discussed Azure Virtual Machines in detail yet; that is coming in the next section. However, I would expect anyone reading this book to have a decent understanding of what a virtual machine is. You should have also created a number of Azure VMs in the previous chapter.

![](img/95043b3df926bcdadadb082b49262037_282_0.png)

A container, on the other hand, is an instance of the image runtime. When an image is deployed to, for example, Azure Container Instances (the host), initially it will only consume space on some form of storage provider, like a hard drive. In that state, it is still an image. Once the image is instantiated, accessed or run, and loaded into memory, it becomes a container. The container becomes a process that comprises the runtime and application code for responding to requests, messages, or web hooks. In summary, a container is a living instance of an image on a host, while an image is a package of code, libraries, and a runtime waiting to be used.

## Container Groups and Multicontainers

A container group, as one would imagine, is a group of containers. So what is a container? Can you answer that without reading forward or backward? A container is an instantiated image or an image that is loaded into memory ready to execute and perform its intended purpose. Therefore, a container group is a group of images running on a host ready to do some work.

It is possible to have multiple container groups, but it is currently only possible to have multiple containers in a single container group when running in a Linux container. This means when you consider a container group as the unit that runs on a virtual machine and the container as a process running on the virtual machine, you can run many containers within the same process. If you know something about IIS, a synonymous scenario is when you run multiple websites within the same application pool, assuming the application pool maps to a single process. See Figure 4.9.

FIGURE 4.9 A representation of multiple containers in a single container group

![](img/95043b3df926bcdadadb082b49262037_284_0.png)

## FIGURE 4.11 How to confirm the enablement of BIOS-level virtualization

4. Install Docker from this site:
https://www.docker.com/products/container-runtime

Scroll down, click the Download Docker Engine button, and select the version based on the operating system running on your workstation. In this example, because I am running Windows, download Docker Desktop for Windows. You will need a Docker account to download the free software.

5. Once downloaded, double-click the Docker Desktop Installer. Accept the default options on Windows. Hyper-V and container features must be enabled. If requested, install them and reboot.

## EXERCISE 4.1 (continued)

6. Enter the following commands into PowerShell and then run as the administrator:

```
docker --version
docker run hello-world
docker images
```

The result of those commands should resemble Figure 4.12.

## FIGURE 4.12 Running Docker from PowerShell

![](img/95043b3df926bcdadadb082b49262037_287_0.png)

The first time you run docker run hello-world, you may get an exception because the hello-world image isn’t yet downloaded. Docker will download it for you the first time and then run it for you.

That’s it—your workstation is ready to create images and run Docker containers.

As alluded to in Exercise 4.1, the variety of operating systems that exist such as Linux, Windows, and iOS would make the exercise too repetitive to perform the steps for each one of them. In general, you simply need to get Git and Docker installed and working on your workstation; I am certain this is achievable with the details provided in the Windows-focused exercise.

With newer workstations, BIOS-level virtualization is enabled by default; for older ones it is not. The means for accessing the BIOS of a computer is dependent on the manufacturer of the machine. Although it is common to press F12 or F2 during bootup, it is not a standard activity, nor is the navigation within the BIOS system, as it too is created by the manufacturer.

Before continuing to the creation of a Docker image and running it in a local Docker container, please review these important limitations. These should be understood when choosing to run your application in ACI.
