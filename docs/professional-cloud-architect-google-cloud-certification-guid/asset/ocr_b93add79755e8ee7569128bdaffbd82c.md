# Shared VPC

In this model, we have a single VPC that is shared between different projects. The project where the shared VPC is created is called the host project. The projects that can use the VPC are called service projects. In the following diagram, we can see we have two service projects (Service Project A and Service Project B):

![](bbox=[0.12, 0.22, 0.87, 0.5])

Figure 10.4 – Shared VPC (Source: https://cloud.google.com/vpc/docs/shared-vpc. License: https://creativecommons.org/licenses/by/4.0/legalcode)

To create a shared project, a shared VPC admin role is required (roles/compute.xpnAdmin). The shared VPC admin can further delegate the permissions to the following:

+   - **Network admin**: Full control over networks, excluding firewall rules and SSL certificates
+   - **Security admin**: Control over firewall and SSL certificates

Users in the service project can provision VMs that are in a shared VPC. To be able to do so, they need to have a network user role assigned. Note that it can be assigned either at a project or subnet level.

# VPC network peering

With VPC peering, you can connect two existing VPCs, regardless of whether they belong to the same project or organization. In this scenario, the administration of each VPC stays separated.