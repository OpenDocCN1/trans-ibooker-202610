![](bbox=[0.146, 0.083, 0.856, 0.355])

Figure 2-8. How applications and components work together within the solution space

The customer management domain is required to maintain information about customers and their relationships. The team managing this domain has deep expertise. It knows what relevant data exists, and therefore also knows best what data should be served to other domains. This domain’s inner architecture is decoupled, so all application components within its boundaries can communicate directly using application-specific interfaces and data models. With this approach, data is aligned with the domain context. Because of that, the data automatically inherits the ubiquitous language: a constructed, formalized language, agreed upon by stakeholders and designers from the same domain, to serve the needs of that domain. The distribution of data to other domains is formalized: it has to leave the property via the common driveway and conform to the rules of the road. In other words, when domains communicate with other domains, they use standardized interfaces.

# Capability realizations

When working with a business capability map, it’s essential to acknowledge that some business capabilities can be instantiated several times. Figure 2-9 visualizes the relationship between business capabilities and business capability instances.