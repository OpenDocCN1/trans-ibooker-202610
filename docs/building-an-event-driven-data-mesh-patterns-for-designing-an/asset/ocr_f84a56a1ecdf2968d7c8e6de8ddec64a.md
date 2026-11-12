Event streams are not new. But many of the technological limitations underpinning previous event-driven architectures, such as limited scale, retention, and performance, have largely been alleviated. Modern multitenant event brokers complete with tiered storage can store an unbounded amount of data, removing the strict capacity restrictions that limited previous architectures. Producers write their important business domain data to an event stream, enabling others to couple on that stream and use the data building blocks for their own applications. Finally, consumer applications can in turn create their own event streams to share their own business facts with others, resulting in a standardized communications mesh for all to use.

Data mesh provides us with very useful concepts and language for building out this interconnected central nervous system. Figure 1-1 shows a basic example of what a data mesh could look like.

![](bbox=[0.146, 0.323, 0.853, 0.613])

# Figure 1-1. A very basic Hello Data Mesh implementation

The team that owns operational system Alpha selects some data from their service boundary, remodels it, and writes it to a source-aligned data product, which they also own (we’ll cover data product alignments more in “The Three Data Product Alignment Types” on page 33). The team that owns operational system Beta reads data from this data product into its own service boundary, again remodeling it, transforming it, and storing only what they need.

Meanwhile, a third team connects to Alpha team’s data product and uses it to compose their own aggregate-aligned data product. This same team then uses its aggregate-aligned data product to both power a streaming analytics use case and to write a batch of files to cloud storage, where data analysts will use it to compose reports and power existing batch-based analytics jobs.