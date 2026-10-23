
the maximum number of DTUs is five, the standard is 3000, and the premium is 4000 DTUs. In Exercise 5.2 you created a basic database with five DTUs. There is no conversion from those DTUs directly into the number of CPUs, the amount of memory, or the maximum IOPS required. Instead, consider that the performance you get when running at ten DTUs is double that when running at five DTUs. Running at the maximum of 4000 DTUs, via premium, would be an 800x increase in compute power (CPU, memory, and IOPS) when compared to running with five. When running in the DTU pricing model, the takeaway is that the price is based on the fixed consumption of CPU, memory, and IOPS. You will not get crystal-clear transparency that translates DTUs into compute. You will need to test and tune the DTUs once you migrate, but the behavior will be consistent enough to get to a stable state after some testing.

The DTU pricing model is fixed, which means when your database consumption hits the threshold, the compute gets throttled. This causes latency, and if the application consuming or creating the data cannot handle such a scenario, then there will be exceptions and potentially lost or corrupted data. After some learnings from customer feedback about DTUs, the concept of elastic pools was realized that included a new pricing model named eDTU. eDTUs provide some flexibility when it comes to the reality of the negative impact, which the fixed DTU concept had on the legacy type of applications that didn’t work well when a threshold was hit. Therefore, the option to move a single database into an elastic pool allowed for the dynamic allocation and deallocation of DTUs based on demand.

One of the other problems experienced with DTUs was the bundling of CPU, memory, and IOPS together. What happens if your database needs more of one item than the other? For example, your database can be heavy on IOPS, but not so heavy regarding memory. You might be required to scale to a very high DTU to get the required throughput, costing a significant amount because the CPU and memory increase are bound to it. The vCore pricing model, which is the recommended model, provides two benefits. The first is that you can scale based on compute, memory/storage, and IOPS, which resolves one of the issues with DTU, making vCore a bit more cost effective. And while we are in the context of costs, the second benefit is that the charging is a bit more transparent. When compared to DTU, you simply had to scale if your database was being throttled. You wouldn’t really know if the reason was CPU, memory, or IO bound. Your only option was to increase the number of DTUs. However, with vCore, you can increase the number of cores that get allocated to your database without increasing other items.

The final pricing model is called serverless, which you may recall mentioned in the previous chapter in the context of compute in the context of FaaS. That concept is the same for an Azure SQL database. In Exercise 5.2 you created the cheapest possible tier and cost model possible. It comes with a fixed monthly price, regardless of its use. As per the serverless model, there is no compute allocated when the product is not being used, and therefore you are not charged. You are charged a specific amount of compute and storage costs based only on its usage and consumption.

So far, you have read about the different Azure SQL products: Azure SQL, SQL managed instances, and SQL VMs. You also learned the differences between DTU/eDTU and vCore-based costing. Let’s now take a look into the different available service pricing tiers. The pricing tiers are separated into two pricing categories based on the pricing models of DTU and vCores. General Purpose (w/ serverless), Business Critical, and Hyperscale are priced using the vCore model. Basic, standard, and premium are priced using DTUs. The basic tier is the most cost-friendly option, and it is the one that you created in Exercise 5.2. For development, non-mission-critical applications or just to learn and have fun, this is a valid option. It’s similar to standard and premium where you have the same PaaS offering and basic but can get more storage capacity and DTUs. For more details about DTU service pricing tiers, please review Table 5.4.

## TABLE 5.4 Basic, Standard, and Premium DTU tiers

|  | Basic | Standard | Premium |
|---|---|---|---|
| DTUs | 5 | 10 to 3000 | 125 to 4000 |
| Database size | 100MB to 2GB | 100MB to 250GB | 100MB to 1TB |
| Read-only replicas | × | × | ✓ |
| Run demanding workloads | × | ✓ | ✓ |
| High I/O workload | × | × | ✓ |
| Secondary DB | × | 1 | Active 0 to 4 |

Having a read-only replica of your data is useful from a failover perspective as well as for performance. From a failover perspective, consider a scenario in which the primary database instance goes down; you could make the read-only instance writable, redirect all the clients to it, and make it the primary database. This may be a faster recovery than troubleshooting and trying to bring the original primary database back online. From a performance perspective, you can tunnel clients that run reports to a read-only database instance. Routing the execution of reports, which typically have large datasets, complicated SQL queries, and extended date ranges, away from OLTP instances, would likely reduce load on the primary database. This would save compute on the primary database for managing OLTP capabilities. It would be hard to define in absolute precision what a demanding workload is, as referred to in Table 5.4. However, it wouldn’t be hard to conclude that five DTUs don’t provide a production-worthy amount of compute power, when compared to the other two tiers. While standard and premium allow for a high number of DTUs, only premium is designed for high I/O workloads. Premium also includes an active secondary data replication mechanism. This mimics the behavior of what is often referred to as a hot standby. This hot standby is not a read-only instance and wouldn’t have clients making a connection to it for any reason. It would act as a failover instance in case the primary instances fails. Table 5.5 shows some details about vCore-based service pricing tiers.

## TABLE 5.5 General Purpose, Business Critical, and Hyperscale vCore Tiers

|  | General Purpose | Business Critical | Hyperscale |
|---|---|---|---|
| Availability | 99.99% | 99.99% to 99.995% | 99.95% to 99.99% |
| Single database | ✓ | ✓ | ✓ |
| Elastic pool | ✓ | ✓ | × |
| Managed instance | ✓ | ✓ | × |
| Database size | 5GB to 4TB<br>32GB to 8TB | 5GB to 4TB<br>32GB to 4TB | Up to 100TB |
| Compute size | 1 to 80 vCores | 1 to 80 vCores | 1 to 80 vCores |
| In memory OLTP | × | ✓ | × |
| Read-only replicas | 0 | 1 | 0 to 4 |
| Serverless | ✓ | × | × |
| Backups | 7 to 35 days | 7 to 35 days | 7 days |
| Autoscaling | ×** | × | ✓* |

* Hyperscale supports the autoscaling of the data storage up to 100TB. The ability to load and access data in memory increases performance as it eliminates the requirement to read the data from disk. This feature, in-memory OLTP, is available only with the Business Critical tier.

** Serverless is supported only via the General Purpose tier and will autoscale the allocated compute resource. When configuring serverless, you choose the maximum number of vCores, which can be between 1 and 16, and the minimum, which can be between 0.5 and 1.

General Purpose is the tier that offers the most cost friendly option. Although the price is much greater than basic/DTU, you get the added benefits of vCore costing, backups, and greater database sizes to name a few of the compared benefits. It is up to you to decide whether that is enough to justify the associated costs. The Business Critical tier is targeted for the lift and shift of OLTP enterprise databases, which require high resilience to failures and require great responsiveness and availability. Hyperscale is, again, exactly what the name implies. Focused also on OLTP databases models with a need for fluid, high-frequency autoscaling. From an availability perspective, you may notice that Business Critical and Hyperscale have a range of possible values, 99.99% to 99.995% and 99.95% to 99.99%, respectively. To achieve the top end SLA availability, you must add additional replicas or zone redundancies.
