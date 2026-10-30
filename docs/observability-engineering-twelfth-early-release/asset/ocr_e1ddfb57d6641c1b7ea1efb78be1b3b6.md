Distributed systems today require an alternative approach. Let’s examine why approaches to monitoring a monolith break down at scale and what we can do differently.

# Threshold Alerting Is for Known Unknowns Only

Anticipating failure modes is much easier in self-contained systems than in distributed systems. So, adding monitoring that expects each of those known failure modes to occur seems logical. Operations teams supporting self-contained production systems can write specific monitoring checks for each precise state.

However, as systems become more complex, they create a combinatorial explosion of potential failure modes. Ops teams accustomed to working with less complex systems will rely on their intuition, best guesses, and memories of past outages to predict any possible system failures they can imagine. While that approach with traditional monitoring may work at smaller scales, the explosion of complexity in modern systems requires teams to write and maintain hundreds, or even thousands, of precise state checks to look for every conceivable scenario.

That approach isn’t sustainable. The checks aren’t maintainable, often the knowledge isn’t transferable, and past historical behavior doesn’t necessarily predict failures likely to occur in the future. The traditional monitoring mindset is often about preventing failure. But in a distributed system with hundreds or thousands of components serving production traffic, *failure is inevitable.*

Today, engineering teams regularly distribute load across multiple disparate systems. They split up, shard, horizontally partition, and replicate data across geographically distributed systems. While these architectural decisions optimize for performance, resiliency, and scalability, they can also make systemic failures impossible to detect or predict. Emergent failure modes can impact your users long before the coarse synthetic probes