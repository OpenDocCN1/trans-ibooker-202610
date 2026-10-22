in object-oriented systems. In microservices, you can exploit an analogous approach using the API Gateway pattern.⁶ An API Gateway essentially acts as a single entry point for all client requests, as shown in Figure 9-4. It insulates clients from the underlying architecture of the microservices that implement the application functionality. Now, if you refactor your underlying APIs or even choose to deploy on a radically different platform such as a private cloud, clients are oblivious to changes.

![](bbox=[0.18, 0.29, 0.85, 0.87])

Figure 9-4. The API Gateway Pattern