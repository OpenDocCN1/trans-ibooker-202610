Each site of the peering is set up independently and peering is only established once both sides are configured. The project owner/editor and network admin roles are allowed to perform the configuration. It is important to remember that the CIDR prefixes cannot overlap between peering VPCs’ subnets. Once peering has been established, every internal IP becomes accessible across peered networks. Multiple peers can be created per VPC, but transitive peering is not supported. This means that only connectivity between directly peered VPCs is allowed. You cannot bridge the traffic between two VPCs via another VPC. To understand this better, take a look at the following diagram:

![](bbox=[0.3, 0.26, 0.65, 0.5])

Figure 10.5 – VPC peering

In this scenario, VPC1 is peered with VPC2 and VPC3. There is no communication between VPC2 and VPC3 until peering between those two is configured. Note that peering can also be established with a shared VPC.

# Choosing between shared VPC and VPC peering

There are two main factors that will allow you to choose between a shared VPC and VPC peering, as follows:

+   - If VPCs exist in different organizations, choose VPC peering.
+   - If you need a shared governance model, choose a shared VPC.

> **Important Note**
> In real-life scenarios, you might have more detailed requirements for your cross-VPC communication, so we encourage you to do further reading on the subject.