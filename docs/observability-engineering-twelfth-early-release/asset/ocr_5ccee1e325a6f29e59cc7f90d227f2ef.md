and large, the debugging practices developed decades ago with nascent monitoring tools are still the same as those used by many engineering teams today—even though the systems they manage are infinitely more complex. Observability tools were born out of sheer necessity, when traditional tools and debugging methods simply were not up to the task of quickly finding deeply hidden and elusive problems.

This chapter will help you understand what “observability” means, how to determine if a software system is observable, why observability is necessary, and how observability is used to find problems in ways that are not possible with other approaches.

# # The Mathematical Definition of Observability

The term “observability” was coined by engineer Rudolf E. Kálmán in 1960. Since then, it has grown to mean many different things in different communities. Let’s explore the landscape before turning to our own definition for modern software systems.

In his 1960 paper, Kálmán introduced a characterization he called *observability* to describe mathematical control systems.¹ In [control theory](#), observability is defined as a measure of how well internal states of a [system](#) can be inferred from knowledge of its external outputs.

This definition of observability would have you study observability and controllability as mathematical duals, along with sensors, linear algebra equations, and formal methods. This traditional definition of observability is the realm of mechanical engineers and those who manage physical systems with a specific end state in mind.

If you are looking for a mathematical and process engineering oriented textbook, you’ve come to the wrong place. Those books definitely exist, and any mechanical engineer or control systems engineer will inform you (usually passionately and at great length) that observability has a formal meaning in traditional systems engineering terminology.