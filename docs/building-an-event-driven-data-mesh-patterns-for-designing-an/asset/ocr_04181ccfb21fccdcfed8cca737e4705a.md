# Data quality classifications

The quality of the data provided by the data product should also be categorized, similar to the approach of the SLA tier system. One choice is to leverage the medallion classifications of bronze, silver, and gold commonly used in data lake architectures. Let's look at each classification:

# Bronze

Unstructured and raw data that is untransformed from the original source format. May be strongly coupled on the internal data model of the source system, and may also contain fields that need to be sanitized or scrubbed. May also include data that is well-structured and defined, but for which quality is intermittent or the data owners simply cannot provide a higher guarantee.

# Silver

Well-structured data with strong typing and typically sanitized and standardized. Usually denormalized to be sufficiently useful as is, with the most common foreign-key relationships having been joined and resolved to provide ease of use to consumers. Type-checking and constraints have been applied to ensure a minimum data quality (e.g., 99.99% of events pass quality checks). The context of the event is clearly defined and documented, as are the type checks and constraints. If a consumer wants to impose their own further, tighter constraints on the data, they would do best to communicate with the data product owner to evaluate options.

# Gold

The highest level of quality. Often referred to as “authoritative,” data products with the gold level of quality are meant to be relied on without reservation. Data products are rigorously tested and monitored, with type-checking and constraints exceeding that of the silver quality level (e.g., 99.9999% of events pass quality checks). Gold data products are often more complex, built up by significant aggregations and transformations that offer significant value, and would be quite difficult for consumers to replicate on their own.

![](bbox=[0.175, 0.69, 0.255, 0.77])

Data quality classification is separate from the data product alignments (source-aligned, aggregate-aligned, and consumer-aligned), and is concerned only with data quality.

You are free to select alternative classification models as you see fit. The important part is that your data mesh users must be able to easily understand and apply the modeling to their own data products, preserving a common understanding between producers and consumers of the data.