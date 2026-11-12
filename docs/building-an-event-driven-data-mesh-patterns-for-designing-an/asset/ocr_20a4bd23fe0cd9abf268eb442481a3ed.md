# Extending the Self-Service Platform for Nonstreaming Data Products

You’ll need to choose which nonstreaming data products to incorporate into your platform based on your unique use cases. “Data Products Are Multimodal” on page 31 introduced the multiple modes that a data product may have, including batch-computed files, request-response APIs, and event streams. But what counts as a separate data product and what counts as a _mode_ of a single data product? How do you represent it in your self-service platform? Let’s take a look at a few factors that influence our options:

# Ownership

The person with the role of data product owner is the sole individual who can own a data product. There cannot be multiple owners. If a single data product is to be made available via multiple modes, there must be a common owner that can guarantee support and consistency for each of the modes.

# Technical boundaries

“Change-Data Capture ”on page 179 is very useful for extracting events from databases, particularly when the database owner is not willing or able to provide the business data as a natively produced event stream. In this case, the CDC stream is a separate data product with ownership that is separate from any request-response data product provided by the source database owners. Keep in mind that if the owner of the transaction system is not willing or able to work with the CDC stream owner, you will have at best a Band-Aid solution prone to failed SLAs and broken data models. It’s essential to work together to maintain data product continuity and schema compatibility whenever possible.

# Alignment and composition logic

A batch of 60-minute sales aggregates derived from a stream of 1-minute sales aggregates are two separate data products, despite both being aggregations of the same data.

A multimodal data mesh should identify the relationships between data products, presented either as modes of the same product or via their relationships to other products. For example, you may choose to put SQL-queryable and event streaming as two modes of a single data product. The metadata entry, as found through your self-service discovery tooling, would look something like Figure 9-6.

Regardless of how you display your batch-created data products, you’ll need to ensure that you have first-class support for discovery, compute, and publishing. There are no shortcuts or easy outs when integrating multiple data product modes, which is why it’s always easier to start with as few as possible and expand your selection only as necessary.