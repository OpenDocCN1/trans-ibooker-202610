
Examples of instance types from various providers:

- *AWS instance m5.xlarge*: An “m” family instance of size “xlarge” in the “5th” generation
- *Azure instance standard_D2_v5*: A “D” series VM of size “2” in the “5th” generation
- *Google Cloud instance n2-standard-128*: An “N2” family instance of size “standard” with “128 CPUs”

Many commitment-based discounts are purchased to match a particular resource type’s usage parameters. This means you would need to make potentially hundreds of specific commitments to cover all the various sizes of instances you use, even if you use the same family of instances widely. To make this process easier, cloud service providers offer instance size flexibility (ISF) on some of their reservations.

Within a specific family, you can choose from a range of server instance sizes (different amounts of vCPU and memory), with each increase in size being proportionally more expensive. For example, on AWS, two matching large instances are the same price as one extra-large instance of the same type (e.g., two m5.large instances cost the same as one m5.xlarge instance). So it makes sense that two purchased large RIs can apply their discount to one extra-large server instance, and vice versa.

The one caveat to this is in the area of software license fees: two medium instances are not the same as one large instance if there are software license fees involved. With two mediums, you need twice the licenses than you’d need for one large. So AWS excludes ISF from servers with licensed software, while Azure charges you separately for the software license costs with reservations (applying a discount to the compute hours only). Note that when you pair reservations with Azure Hybrid Benefit, the software license costs are also zeroed out.

Due to the way Google Cloud charges, they naturally do ISF by combining all instances’ vCPU and memory counts within the same machine family. CUDs apply to the combined total instead of against the individual discrete instances.

ISF is very helpful for engineering teams who wish to use a certain family of compute or database services but want the flexibility of changing the size of the instances they are launching as the needs of their workloads change. ISF gives these teams much more flexibility and dramatically reduces the workload on the FinOps team because no action is required to move the coverage to different instance sizes—it happens automatically.

## Conversions and Cancellations

Some commitment-based discount programs offer the ability to convert commitments or exchange them for another type. AWS Convertible reservations offer lower discount rates than reservations that cannot be exchanged, but allow users the option of exchanging the reservation for a different family or a different size (if ISF does not apply). The ability to convert reservations lowers the risk of ending up with reservations that don’t match your resource usage. If resources get updated so they no longer match, you can exchange the reservation for one that does. The ability to exchange or convert reservations is typically restricted to the region in which they were originally purchased.

Keep in mind also that converting RIs from one type to another requires analysis and work on the part of the FinOps team, so consider this cost in the overall equation when considering your options.

It should be noted here that AWS SPs, as of this writing, offer the same discounts that AWS RIs offer, but with convertibility built in. A compute SP, for example, covers compute instances in multiple regions and in any family without requiring any analysis or conversion to modify them, at the same discount as a convertible RI in one region for one type of family. SPs are discussed further in the section on AWS.

Azure does not offer convertible RIs, but it does allow users to cancel and exchange a certain dollar value of reservations each year. Sometimes this entails a fee, and usually it also requires the new reservation to start the clock over again from the time of the exchange. Azure SPs are now available as an alternative to reservations and function similarly to AWS SPs, offering much more flexibility than Azure RIs.

Google Cloud offers the ability to split or merge commitments. Splitting allows you to redistribute resources across your projects or organization. Merging commitments makes it easier to manage the discounts as a single entity with one expiration date. Google Cloud also now offers Flexible CUDs, bringing similar flexibility as SPs at AWS or Azure.

In general, the conversions you make must be equal or greater in value and duration. So if you lower your usage of the cloud service provider, you can’t exchange reservations if doing so results in a lower overall commitment.

One additional option AWS users have is the ability to sell certain types of reservations with a significant amount of time remaining on a secondary marketplace. Typically this can only be done with Standard, or nonconvertible, RIs with more than half their life remaining.

## Overview of Usage Commitments Offered by the Big Three

Table 17-1 shows the big three cloud providers and compares their offerings.

Table 17-1. Comparison of reservation offerings across the top three cloud service providers

|  | AWS | Google | Azure |
|---|---|---|---|
| Program name | Reserved Instances/Savings Plans | Committed Use Discounts/ Flexible CUDs | Reservations/Savings Plans |
| Payment model | All up front, partial, no up front (different discount rates) | No up front | All up front, monthly (same price) |
| Term | One or three years | One or three years | One, three, or five years |
| Instance size flexibility | Yes | N/A | Yes |
| Convert or exchange | Standard, No Convertible, Yes | Yes, splitting and merging | Yes |
| Cancelable | No; however, some RIs can be sold via a marketplace | No | Yes, up to a yearly value limit |
| Sustained use discounts | No | Yesᵃ | No |

ᵃ SUDS are being phased out for new instance families, because customers favor the predictability of CUDs.

You can see that the big three cloud service providers’ offerings are very similar, with similar term length and size flexibility. Where the discount offerings differ is largely driven by their specific methods of applying the reservation and method of billing for the discounted cloud resources.

## Amazon Web Services

AWS was the first to offer RIs back in 2009. Thus, they have by far the most complex reservation programs. However, this also means that AWS is the most flexible and configurable. Reservations are available for EC2 instances, RDS databases, Redshift nodes, ElastiCache nodes, OpenSearch nodes, and DynamoDB reserved capacity.

DynamoDB reserved capacity is different from the other reservations. With DynamoDB, you reserve a set amount of reads and writes to this service. The remaining reservation types each have slightly different features, but what’s common to all of them is that you purchase them in one- or three-year terms. You can pay either nothing up front, make a partial payment up front, or pay all up front for the reservation, with each increase in up-front payment giving you a few more discount percentage points. When purchasing RIs, you buy one or more with the same parameters in what AWS calls a lease.

![](img/4d4736bc293fd892d1da81802d0f9520_289_0.png)

You can think of the difference between the total amount you pay for a resource covered by an RI using partial or no up-front payment models and the all up-front payment model as the interest AWS charges you to lend you the up-front payment. By figuring this out (it’s different for each resource’s RI) you can compare this imputed interest rate to your internal cost of capital and see whether it’s better to invest the cash in buying up front or taking the lower discount and paying less (or nothing) up front.

RIs are confusingly named, since you aren’t actually reserving a specific instance. Nor can a reservation be guaranteed to apply to a specific resource. Remember, RIs are more like coupons you purchase and apply against relevant resources nondeterministically.

RIs are often thought of as a percentage discount applied to your resource’s cost. However, it is also correct to say the RI has a fixed rate that’s a certain amount cheaper than the on-demand rate at purchase. While this might sound like the same thing, it’s important to understand that if AWS reduces the on-demand rate, your existing RI rates remain the same during the one- or three-year term.

Price drops may affect your decision about which RI configuration to purchase—we’ll get into that later in the chapter. It’s worth noting that an analysis of historic AWS price drops showed that AWS doesn’t typically reduce the rate of on-demand enough to undercut the three-year RI price.

### What Does an RI Provide?

AWS reservations have two parts: a rate reduction benefit and in one case, a capacity reservation for the type of resource purchased in a specific availability zone. The majority of RI purchases are made to get a rate reduction. But some companies also want a capacity reservation as well, ensuring that the number of resources they reserve of a particular type in a very particular place are available when they want to run them.

Over the years, we’ve seen the capacity reservation become a less important aspect of RI purchasing as AWS has scaled their infrastructure. However, some customers running massive, elastic infrastructures still optimize for capacity as well. More recently, AWS and Azure have offered On-Demand Capacity Reservations (ODCRs), which allow you to perform capacity reservations separately from RIs. Google Cloud offers a similar concept called Compute Engine zonal resources, which provide a very high level of assurance in obtaining capacity for a specific VM type. Note that when you’re using on-demand capacity reservations in combination with AWS RIs, it’s essential to set your RIs as regional so the RIs can discount the capacity reservation.
