Furthermore, the integration model’s decoupling allows the upstream bounded context to simultaneously expose multiple versions of the published language, allowing the consumer to migrate to the new version gradually (see Figure 4-7).

![](bbox=[0.14, 0.2, 0.86, 0.52])

Figure 4-7. Open-host service exposing multiple versions of the published language

# Separate Ways

The last collaboration option is not to collaborate at all. This pattern can arise for different reasons, in cases where the teams are unwilling or unable to collaborate. We’ll look at a few of them here.

# Communication Issues

A common reason for avoiding collaboration is communication difficulties driven by the organization’s size or internal politics. When teams have a hard time collaborating and agreeing, it may be more cost-effective to go their separate ways and duplicate functionality in multiple bounded contexts.