container is often referred to as the runtime. The following are a few benefits for running application code in containers:

+   - Containers are portable.
+   - Containers are lightweight.
+   - Containers are flexible.
+   - Containers are scalable.

From a portable perspective, containers allow a developer or release manager to have confidence that the code in the container will run anywhere, on a local development environment, on a corporate on-premise physical server, or in the cloud. It will run anywhere without making any configuration or coding changes because it has no system dependencies. A container is lightweight because it reuses the operating system of the host; there is no need for an operating system to be deployed along with the container. It is flexible because all kinds of programs—small, large, simple, or complex—can run in a container. They are scalable, which means the number of servers that can simultaneously run the container can be added to provide more compute power when usage increases.

When a container package is deployed to ACI, it receives a public-facing IP address and domain name with the extension *<region>.azurecontainer.io, where * is the name of the ACI container that must be unique for the given region. A region, as covered in detail in Chapter 3, is the geographical location that the ACI will be installed into. This is selected during the container’s creation. Regions are, for example, northeurope or southcentralus. Keep in mind that ACI is not yet available in all Azure regions; however, the deployment globally is ongoing and will reach worldwide scale in the short term.

ACI offers support for containers running on either Windows or Linux. The container concept has been mostly focused on the open source community and therefore Linux, so you will find the number of supported features for Linux images is greater than that currently for Windows. Table 4.1 lists the currently supported Windows base images on ACI.

# TABLE 4.1 ACI-Supported Windows Images

| Server Version | Edition | Version |
|---|---|---|
| Windows Server 2016 | Nano Server | 10.0.14393.* |
| Windows Server 2016 | Server Core | 10.0.14393.* |
| Windows Server 2019 | Nano Server | 10.0.17763.* |
| Windows Server 2019 | Server Core | 10.0.17763.* |
| Windows Server 2019 | Windows | 10.0.17763.* |