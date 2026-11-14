
*   [“NoSQL: What’s in a Name”](https://oreil.ly/z0xZH) by Eric Evans
*   [“Test Data Quality at Scale with Deequ”](https://oreil.ly/XoHFL) by Dustin Lange et al.
*   [“The What, Why, and When of Single-Table Design with DynamoDB”](https://oreil.ly/jOMTh) by Alex DeBrie

^([1](#ch05.html#ch01fn47-marker)) Keith D. Foote, “A Brief History of Non-Relational Databases,” Dataversity, June 19, 2018, [`https://oreil.ly/5Ukg2`](https://oreil.ly/5Ukg2).

^([2](#ch05.html#ch01fn48-marker)) Nemil Dalal’s excellent series on the [history of MongoDB](https://oreil.ly/pEKzk) recounts some harrowing tales of database abuse and its consequences for fledgling startups.

^([3](#ch05.html#ch01fn49-marker)) Martin Kleppmann, *Designing Data-Intensive Applications* (Sebastopol, CA: O’Reilly, 2017), 49, [`https://oreil.ly/v1NhG`](https://oreil.ly/v1NhG).

^([4](#ch05.html#ch01fn50-marker)) Aashish Mehra, “Graph Database Market Worth $5.1 Billion by 2026: Exclusive Report by MarketsandMarkets,” Cision PR Newswire, July 30, 2021, [`https://oreil.ly/mGVkY`](https://oreil.ly/mGVkY).

^([5](#ch05.html#ch01fn51-marker)) For one example, see Michael S. Mikowski, “RESTful APIs: The Big Lie,” August 10, 2015, [`https://oreil.ly/rqja3`](https://oreil.ly/rqja3).

^([6](#ch05.html#ch01fn52-marker)) Martin Fowler, “How to Move Beyond a Monolithic Data Lake to a Distributed Data Mesh,” MartinFowler.com, May 20, 2019, [`https://oreil.ly/TEdJF`](https://oreil.ly/TEdJF).

^([7](#ch05.html#idm45075125319424-marker)) Whether *exactly once* is possible is a semantical debate. Technically, exactly once delivery is impossible to guarantee, as illustrated by the [Two Generals Problem](https://oreil.ly/4VL1C).

^([8](#ch05.html#ch01fn53-marker)) James Denmore, *Data Pipelines Pocket Reference* (Sebastopol, CA: O’Reilly), [`https://oreil.ly/8QdkJ`](https://oreil.ly/8QdkJ). Read the book for more information on how a data contract should be written.

# Chapter 6. Storage

Storage is the cornerstone of the data engineering lifecycle ([Figure 6-1](#ch06.html#storage_plays_a_central_role_in_the_dat)) and underlies its major stages—ingestion, transformation, and serving. Data gets stored many times as it moves through the lifecycle. To paraphrase an old saying, it’s storage all the way down. Whether data is needed seconds, minutes, days, months, or years later, it must persist in storage until systems are ready to consume it for further processing and transmission. Knowing the use case of the data and the way you will retrieve it in the future is the first step to choosing the proper storage solutions for your data architecture.

![](assets/fode_0601.png)

We also discussed storage in [Chapter 5](#ch05.html#data_generation_in_source_systems), but with a difference in focus and domain of control. Source systems are generally not maintained or controlled by data engineers. The storage that data engineers handle directly, which we’ll focus on in this chapter, encompasses the data engineering lifecycle stages of ingesting data from source systems to serving data to deliver value with analytics, data science, etc. Many forms of storage undercut the entire data engineering lifecycle in some fashion.

To understand storage, we’re going to start by studying the *raw ingredients* that compose storage systems, including hard drives, solid state drives, and system memory (see [Figure 6-2](#ch06.html#raw_ingredientscomma_storage_systemscom)). It’s essential to understand the basic characteristics of physical storage technologies to assess the trade-offs inherent in any storage architecture. This section also discusses serialization and compression, key software elements of practical storage. (We defer a deeper technical discussion of serialization and compression to [Appendix A](#app01.html#a_serialization_and_compression_technic).) We also discuss *caching*, which is critical in assembling storage systems.

![](assets/fode_0602.png)

Next, we’ll look at *storage systems*. In practice, we don’t directly access system memory or hard disks. These physical storage components exist inside servers and clusters that can ingest and retrieve data using various access paradigms.

Finally, we’ll look at *storage abstractions*. Storage systems are assembled into a cloud data warehouse, a data lake, etc. When building data pipelines, engineers choose the appropriate abstractions for storing their data as it moves through the ingestion, transformation, and serving stages.

## Raw Ingredients of Data Storage

Storage is so common that it’s easy to take it for granted. We’re often surprised by the number of software and data engineers who use storage every day but have little idea how it works behind the scenes or the trade-offs inherent in various storage media. As a result, we see storage used in some pretty...interesting ways. Though current managed services potentially free data engineers from the complexities of managing servers, data engineers still need to be aware of underlying components’ essential characteristics, performance considerations, durability, and costs.

In most data architectures, data frequently passes through magnetic storage, SSDs, and memory as it works its way through the various processing phases of a data pipeline. Data storage and query systems generally follow complex recipes involving distributed systems, numerous services, and multiple hardware storage layers. These systems require the right raw ingredients to function correctly.

Let’s look at some of the raw ingredients of data storage: disk drives, memory, networking and CPU, serialization, compression, and caching.

### Magnetic Disk Drive

*Magnetic disks* utilize spinning platters coated with a ferromagnetic film ([Figure 6-3](#ch06.html#magnetic_disk_head_movement_and_rotatio)). This film is magnetized by a read/write head during write operations to physically encode binary data. The read/write head detects the magnetic field and outputs a bitstream during read operations. Magnetic disk drives have been around for ages. They still form the backbone of bulk data storage systems because they are significantly cheaper than SSDs per gigabyte of stored data.

On the one hand, these disks have seen extraordinary improvements in performance, storage density, and cost.^([1](#ch06.html#ch01fn54)) On the other hand, SSDs dramatically outperform magnetic disks on various metrics. Currently, commercial magnetic disk drives cost roughly 3 cents per gigabyte of capacity. (Note that we’ll frequently use the abbreviations *HDD* and *SSD* to denote rotating magnetic disk and solid-state drives, respectively.)

![](assets/fode_0603.png)

IBM developed magnetic disk drive technology in the 1950s. Since then, magnetic disk capacities have grown steadily. The first commercial magnetic disk drive, the IBM 350, had a capacity of 3.75 megabytes. As of this writing, magnetic drives storing 20 TB are commercially available. In fact, magnetic disks continue to see rapid innovation, with methods such as heat-assisted magnetic recording (HAMR), shingled magnetic recording (SMR), and helium-filled disk enclosures being used to realize ever greater storage densities. In spite of the continuing improvements in drive capacity, other aspects of HDD performance are hampered by physics.

First, *disk transfer speed*, the rate at which data can be read and written, does not scale in proportion with disk capacity. Disk capacity scales with *areal density* (gigabits stored per square inch), whereas transfer speed scales with *linear density* (bits per inch). This means that if disk capacity grows by a factor of 4, transfer speed increases by only a factor of 2. Consequently, current data center drives support maximum data transfer speeds of 200–300 MB/s. To frame this another way, it takes more than 20 hours to read the entire contents of a 30 TB magnetic drive, assuming a transfer speed of 300 MB/s.
