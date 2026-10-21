producing high-quality models and systems that work with the goals of rapid iteration of DevOps and MLOps.

Such a framework would be similar to APM but would apply to ML models and account for the nuances and differences between these models and traditional applications. This framework can be called *model performance management* (MPM). Model performance is not only reliant on metrics but also on how well a model can be explained when something eventually goes wrong. This is known as *model explainability*. Just as APM aims at providing ops teams with more visibility into the entire application development process, MPM allows those teams to monitor models as they are being trained, validated, and deployed. In this chapter, we will discuss the need for such a framework before delving deeper into the nuances and stages involved in the ML life cycle and how MPM and model explainability fit into it.

# DevOps and Application Performance Management

Let’s take a step back and talk about why DevOps and APM came to be and what they are.

# DevOps

DevOps, or development operations, is a term used to describe the combination of cultural practices, business processes, and toolchains that are used to achieve rapid iteration and high velocity. The goal is to innovate quickly and catch problems and issues as soon as they arise. There are many DevOps frameworks and tools out there, and each has its pros and cons, but at the center of it all is an overarching philosophy that unifies the practices in some ways across companies and domains.

One tenet of DevOps is system observability and, in turn, monitoring. Monitoring is achieved through the use of metrics and logs that enable us to track what is happening inside an application. There are tools that do