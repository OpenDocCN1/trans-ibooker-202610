We have not yet seen a standard approach to allocating the excess, allocatable node capacity. One option is to have the team managing the server instance itself cover the cost; another is to spread the cost of the allocatable node capacity equitably between all the containers within the node based on their respective proportion of usage of the overall node.

![](bbox=[0.14, 0.22, 0.25, 0.32])

Reserved capacity is a soft limit, and pods are able to exceed its set limits if there is additional allocatable capacity remaining within the node. As such, the amount of capacity that a pod consumes can be more than 100% of its reserved capacity. This means that the pod's portion of the overall node's costs would increase during the period of use, and less of the unreserved node capacity would be left unused.

This leaves the question of who should pay for the excess capacity: the team managing the cluster or the teams who own the workloads running on the server instances?

Having spent time with organizations that are successfully allocating their container costs, we've identified two main strategies for assigning these unallocated costs when they exist:

# Assigning the idle costs to a central team (recommended option)

Since you need an allocation strategy to make sure that your cluster costs can be 100% allocated, some organizations take those excess capacity costs and assign them to the team that runs the cluster. These teams have their budget and forecasts on cluster costs, so by mapping the excess capacity costs to them, you incentivize the group to optimize the container scheduling, reduce the size of the cluster as permitted by the workload, and keep cluster waste to a minimum, thereby reducing those excess capacity costs.

> The team running and administering the cluster gets visibility into the allocatable costs on cluster/node level. In most cases the actual usage through the deployed services/pods is dependent on the resource requests that are set by the actual users like the platform team running the platform services for everyone and the developer teams with their respective applications. The platform team is positioned best to rightsize the cluster, switch to other instance types, or set up a cluster autoscaler to overall reduce the allocatable costs by better sizing the cluster, which increases efficient utilization of the cluster's resources.
> 
> —David Sterz, Mindcurv, Container Cost Allocation Working Group lead

# Distributing the idle costs to the teams using the cluster (alternative option)

The second strategy is to distribute the excess capacity costs to the teams running containers on the cluster. You determine what gets distributed, and where, by using the same proportion data you use to allocate the costs of the containers.