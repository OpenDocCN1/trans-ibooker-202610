Second, exposing a bounded context’s data in a model that is different from its operational model is the open-host pattern. In this case, the analytical model is an additional published language.

The CQRS pattern makes it easy to generate multiple models of the same data. It can be leveraged to transform the operational model into an analytical model. The CQRS pattern’s ability to generate models from scratch makes it easy to generate and serve multiple versions of the analytical model simultaneously, as shown in Figure 16-15.

![](bbox=[0.12, 0.33, 0.9, 0.62])

Figure 16-15. Leveraging the CQRS pattern to simultaneously serve the analytical data in two different schema versions

Finally, since the data mesh architecture combines the different bounded contexts’ models to implement analytical use cases, the bounded context integration patterns for operational models apply for analytical models as well. Two product teams can evolve their analytical models in partnership. Another can implement an anticorruption layer to protect itself from an ineffective analytical model. Or, on the other hand, the teams can go their separate ways and produce duplicate implementations of analytical models.