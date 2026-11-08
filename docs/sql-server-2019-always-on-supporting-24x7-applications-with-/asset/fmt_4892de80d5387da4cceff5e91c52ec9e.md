
## RPO and RTO

Applications may have different `RPOs` for high availability and for disaster recovery. For example, for reasons of cost or application performance, an `RPO` of zero may be required for a failover within the site. If the same application fails over to a DR data center, however, five or ten minutes of data loss may be acceptable. This is because of technology differences used to implement intra-site availability and inter-site recovery.

The recovery time objective (`RTO`) for an application specifies the maximum amount of time an application can be down before recovery is complete and users can reconnect. When calculating the achievable `RTO` for an application, you need to consider many aspects. For example, it may take less than a minute for a cluster to fail over from one node to another and for the `SQL Server` service to come back up; however, it may take far longer for the databases to recover. The time it takes for databases to recover depends on many factors, including the size of the databases, the quantity of databases within an instance, and how many transactions were in-flight when the failover occurred. This is because all noncommitted transactions need to be rolled back.

Just like `RPO`, it is common for there to be different `RTOs` depending on whether you have an intra-site or inter-site failover. Again, this is primarily due to differences in technologies, but it also factors in the amount of time you need to bring up the entire estate in the DR data center if the primary data center is lost.

The `RPO` and `RTO` of an application may also vary in the event of data corruption. Depending on the nature of the corruption and the HA/DR technologies that have been implemented, data corruption may result in you needing to restore a database from a backup.

If you must restore a database, the worst-case scenario is that the achievable point of recovery may be the time of the last backup. This means that you must factor a hard business requirement for a specific `RPO` into your backup strategy. If only part of the database is corrupt, however, you may be able to salvage some data from the live database and restore only the corrupt data from the restored database.

Data corruption is also likely to have an impact on the `RTO`. One of the biggest influencing factors is if backups are stored locally on the server, or if you need to retrieve them from tape. Retrieving backup files from tape, or even from off-site locations, is likely to add significant time to the recovery process.

### Note

Backups directly to tape from `SQL Server` are deprecated. When this section refers to retrieving backups from tape, it is assuming a tape drive as the target for an enterprise backup solution that your database backups have been offloaded too.

Another influencing factor is what caused the corruption. If it is caused by a faulty IO subsystem, then you may need to factor in time for the Windows administrators to run the check disk command (`CHKDSK`) against the volume and potentially more time for disks to be replaced. If the corruption is caused by a user accidently truncating a table or deleting a data file, however, then this is not of concern.

## Cost of Downtime

If you ask any business owners how much downtime is acceptable for their applications and how much data loss is acceptable, the answers invariably come back as zero and zero, respectively. Of course, it is never possible to guarantee zero downtime, and once you begin to explain the costs associated with the different levels of availability, it starts to get easier to negotiate a mutually acceptable level of service.

The key factor in deciding how many 9s you should try to achieve is the cost of downtime. Two categories of cost are associated with downtime: tangible costs and intangible costs. Tangible costs are usually fairly straightforward to calculate. Let’s use a sales application as an example. In this case, the most obvious tangible cost is lost revenue because the sales staff cannot take orders. Intangible costs are more difficult to quantify but can be far more expensive. For example, if a customer is unable to place an order with your company, they may place their order with a rival company and never return. Other intangible costs can include loss of staff morale, which leads to higher staff turnover, or even loss of company reputation. Because intangible costs, by their very nature, can only be estimated, the industry rule of thumb is to multiply the tangible costs by three and use this figure to represent your intangible costs.

Once you have an hourly figure for the total cost of downtime for your application, you can scale this figure out, across the predicted life cycle of your application, and compare the costs of implementing different availability levels. For example, imagine that you calculate that your total cost of downtime is $2,000/hour and the predicted life cycle of your application is three years. Table [1-2](#394392_3_En_1_Chapter.xhtml#Tab2) illustrates the cost of downtime for your application, comparing the costs that you have calculated for implementing each level of availability, after you have factored in hardware, licenses, power, cabling, additional storage, and additional supporting equipment, such as new racks, administrative costs, and so on. This is known as the total cost of ownership (`TCO`) of a solution. Table 1-2

Cost of Downtime

| Level of Availability || Cost of Downtime (Three Years) || Cost of Availability Solution |
| --- | --- | --- |
| 99% || $525,600 || $108,000 |
| --- | --- | --- |
| 99.9% || $52,560 || $224,000 |
| 99.99% || $5,256 || $462,000 |
| 99.999% || $526 || $910,000 |

In this table, you can see that implementing five 9s of availability saves $525,074 over a two-9s solution, but the cost of implementing the solution is an additional $802,000, meaning that it is not economical to implement. Four 9s of availability saves $520,344 over a two-9s solution and only costs an additional $354,000 to implement. Therefore, for this particular application, a four-9s solution is the most appropriate level of service to design for.

## Classification of Standby Servers

There are three classes of standby solution. You can implement each using different technologies, although you can use some technologies to implement multiple classes of standby server. Table [1-3](#394392_3_En_1_Chapter.xhtml#Tab3) outlines the different classes of standby that you can implement. Table 1-3

Standby Classifications

| Class || Description || Example Technologies |
| --- | --- | --- |
| Hot || A synchronized solution where failover can occur automatically or manually. Often used for high availability. || `Clustering`, `AlwaysOn Availability Groups` (Synchronous) |
| --- | --- | --- |
| Warm || A synchronized solution where failover can only occur manually. Often used for disaster recovery. || `Log Shipping`, `AlwaysOn Availability Groups` (Asynchronous) |
| Cold || An unsynchronized solution where failover can only occur manually. This is only suitable for read-only data, which is never modified. || – |

### Note

Cold standby does not show an example technology because no synchronization is required and, thus, no technology implementation is required.

## Summary

Your application’s level of availability is measured as a percentage of time that the application is available to users. The level of availability is often referred in nines. For example, 99.9% uptime requirement is known as three 9s of availability. The higher the uptime requirement, the higher the cost of implementing the solution. Therefore, the level of uptime that you strive to achieve should be driven by `SLAs` and the cost of downtime.
