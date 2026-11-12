# Can Systems Make Interfaces That Expose Raw Data?

Consider allowing applications to provide raw data and consumption-optimized data simultaneously. This approach benefits data scientists and consumers who want immediate access. Interfaces holding raw data should be marked as private and come without guarantees. They should never be used as production data pipelines.

The principle of no raw data, as discussed in Chapter 2, also applies for external data providers: external parties who operate outside the logical boundaries of your enterprise’s ecosystem. They generally operate in separate, uncontrolled network locations. Ask your ecosystem partners to conform to your standard or apply mediation via an intermediate domain: a consuming domain that acts as an intermediate by abstracting complexity and guaranteeing stable and safe consumption.

# Don’t Conform to Consumers

It’s likely that the same data products will be used repeatedly by different domain teams for a wide range of use cases. So your teams shouldn’t conform their data products to specific needs of (individual) data consumers. The primary design principle should be to maximize domain productivity and promote consumption and reuse.

On the other hand, data products can and do evolve based on user feedback. You should be careful here! It can be tempting for teams to incorporate specific requirements from consumers. But if consumers push business logic into a producing domain’s data products, it creates a strict dependency with the provider for making changes. This can trigger intense cross-coordination and backlog negotiation between domain teams. A recommendation here is to introduce a governance body that oversees that data products aren’t created for specific consumers. This body can step in to guide domain teams, organize walk-in and knowledge-sharing sessions, provide practical feedback, and resolve issues between teams.

# The Dangers of Outsourcing Business Logic

Let me share a personal story. In my previous role, the head of accounting proposed making the International Financial Reporting Standards (IFRS) accounting rules an integral part of all data products. He presumed that summing up calculated outcomes is easier than calculating everything yourself. There are a number of good reasons why you should never outsource your business logic to other domains. For one, it assumes that other domains have in-depth knowledge of your domain. It also requires all domains to simultaneously carry out changes when business logic must be updated. How would you manage this in an environment with tens or hundreds of domains? You’d probably need an army of consultants to orchestrate and manage all of these activities. The correct approach is to keep business logic close to where