While you can go really extreme and map one function per container, it does make sense to bundle several functions that are related and we want to scale together into a single container.

![](bbox=[0.18, 0.25, 0.78, 0.38])

Figure 7.6 One function per project vs multiple functions per project

If you do so, then you will need a way to route events to different functions inside the same `func` project (all the functions that you bundle together will share the same container). This routing mechanism will be then specific for each language but we can see two very common patterns:

+   - Routing based on a CloudEvent attribute (HTTP headers)
+   - Routing based on path, each function can expose a different path where it will receive CloudEvents

![](bbox=[0.22, 0.6, 0.52, 0.75])

A `func` project can contain multiple functions, in this example 3 functions. The project can expose a single endpoint and then route internally using, for example, the CE-Type to decide which function should process a specific CloudEvent. In some cases, it makes a lot of sense to bundle functions together. While these functions will be scaled together, and cannot be scaled independently, this can be a good trade off if the functions tends to be used together to implement more complex logic.

Figure 7.7 Multiple functions per `func` project using internal routing mechanism