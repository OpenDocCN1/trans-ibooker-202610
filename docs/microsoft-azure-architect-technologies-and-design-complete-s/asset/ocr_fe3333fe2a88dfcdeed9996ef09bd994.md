# FIGURE 5.21 Setting the maximum and minimum for autoscaling

![](bbox=[0.13, 0.14, 0.85, 0.46])

If you wanted to scale from a tier that targeted the DTU pricing model, it would be possible, but only manually. You could scale from basic to standard or premium and then, when demand required it, increase the number of allocated DTUs. In the same context, you could consider using an elastic pool that can target either DTUs or vCores and adding the database to the pool.

# SQL Azure Database Server

A SQL database server can contain different tiers and pricing models. It is a virtual grouping of Azure SQL databases that is similar to the construct and purpose of a resource group.

It is an interesting area to pursue, and you should if it interests you. For the exam, the internals of all scaling options are not relevant. Just note that when creating an elastic pool the selection of tier is the primary consideration.

When you decide the pool is a PaaS that is constrained by the offerings within tiers—basic, standard, premium, general purpose, and business critical—hyperscale is noticeably absent. It was interesting to find that in the context of the basic DTU elastic pool pricing model that the minimum eDTUs was 50. This makes sense because it is expected that you would have multiple databases in the pool and would require more than the minimal required via the single database default value of five.