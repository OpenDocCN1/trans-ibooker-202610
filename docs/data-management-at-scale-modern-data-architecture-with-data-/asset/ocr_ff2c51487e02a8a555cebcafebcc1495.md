![](bbox=[0.14, 0.08, 0.86, 0.41])

Figure 2-13. Different patterns are blended together in the emissions and fees domain

![](bbox=[0.175, 0.455, 0.235, 0.535])

A domain aggregate is a cluster of domain objects that can be treated as a single unit. When you have a collection of objects of the same format and type that are used together, you can model them as single object, simplifying their usage for other domains. A domain aggregate shouldn’t be confused with data aggregation, which is the process of grouping data and expressing it in a summary form.

Another scenario, illustrated in Figure 2-14, is when consumers become providers after creating new data—for example, by making predictions about passengers’ travel habits. Before they become providers, these consuming domains use bits of domain logic from other domains combined with their own domain logic to transform the aggregated data into new data products, which might be of interest to other domains as well. Such domains are sometimes called constructor domains because they create something new.