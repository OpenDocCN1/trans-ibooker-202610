# Part III. Observability for Teams

In [Part II](Part II), we examined various technical aspects of observability, how those concepts build on one another to enable the core analysis loop and debugging from first principles, and how that practice can coexist with traditional monitoring. In this part, we switch gears to look at the changes in social and cultural practices that help drive observability adoption across different teams.

[Chapter 10](Chapter 10) tackles many of the common challenges teams face when first starting down the path of observability. How and where you start will always depend on multiple factors, but this chapter recaps many of the techniques we’ve seen work effectively.

[Chapter 11](Chapter 11) focuses on how developer workflows change when using observability. Though we’ve referenced this topic in earlier chapters, here we walk through more concrete steps. You’ll learn about the benefits developers gain by adding custom instrumentation into their code early in the development phase, and how that’s used to debug their tests and to ensure that their code works correctly all the way through production.

[Chapter 12](Chapter 12) looks at the potential that observability unlocks when it comes to using more sophisticated methods for monitoring the health of your services in production. This chapter introduces service-level objectives (SLOs) and how they can be used for more effective alerting.

[Chapter 13](Chapter 13) builds on the preceding chapter by demonstrating why event data is a key part of creating more accurate, actionable, and debuggable alerts than using SLOs based on metrics data.

[Chapter 14](Chapter 14) looks at how teams can use observability to debug and better understand other parts of their stack, like their CI/CD build pipelines. This