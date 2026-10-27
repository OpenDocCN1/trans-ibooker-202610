implementation, described schematically in the Figure 10.4.

# Figure 10.4: Google Distributed Cloud Edge Container Resource Model

![](bbox=[0.122, 0.181, 0.881, 0.563])

# Zone

Zone represents a set of machines sharing a network fabric or a single fault domain. A zone can represent one rack or a number of racks placed in the same location. The GDCE Zone resource is different from GCE Compute Zones. Listing the GCE Compute Zones will not return the GDCE Zones. Zone represents a single failure domain, to deploy fault-tolerant applications with high availability and help protect against failures, users need to deploy applications across multiple Zones linked to the same region.

# Machine

A machine represents a physical server, the machine resource metadata includes which rack it is on and other properties and tags. Machines are read