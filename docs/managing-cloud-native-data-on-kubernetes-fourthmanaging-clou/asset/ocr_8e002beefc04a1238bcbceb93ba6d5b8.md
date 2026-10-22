# CAN ONE OPERATOR RULE THEM ALL?

With Umair Mufti, Product Manager, PortWorx

The number of Kubernetes operators for databases has been continually growing. Database developers want their databases to run on Kubernetes, so to their credit, they are stepping up and developing operators to make it easy for others. This initiative is great, but the problem is that everyone is building operators their own way and solving similar problems with different implementations. As a result, there is no uniformity between operators for stateful workloads.

The question we have to reckon with is how specialized we expect end-users to be. Because of the popularity of cloud-native, microservice architectures, application developers now expect polyglot persistence: to run a relational database right next to a graph database or a key-value store. This gives cluster administrators the requirement to provide different types of databases while maintaining the operational simplicity of a single platform.

No Kubernetes admin wants to be maintaining 10 or 15 different operators on their cluster. The point of Kubernetes is the ease of operations when deploying applications, monitoring them on day two, and making lifecycle management simpler. As soon as you have the maintenance overhead of managing operator lifecycles, you are already broken. Multiply that 10 or 15 times over, and you are completely at odds with what Kubernetes is trying to provide. The only way out of this situation is to reduce the number of operators. Could there be a single operator for all our databases or stateful workloads? Let’s explore.

The operator pattern is simply a design pattern for running stateful workloads in Kubernetes, just as the Model View Controller framework is a pattern for user-facing applications. Various web frameworks such as Angular, Vue and React use the MVC pattern, but they all implement the pattern in different ways, and your code will vary based on the