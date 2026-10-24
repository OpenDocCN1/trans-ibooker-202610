There are two types of capabilities in a microservice system:

+   - A *business capability* is something the system does that contributes to the purpose of the system, like keeping track of users’ shopping carts or calculating prices. A good way to tease apart a system’s separate business capabilities is to use domain-driven design.
+   - A *technical capability* is one that several other microservices need to use—integration to some third-party system, for instance. Technical capabilities aren’t the main drivers for breaking down a system to microservices; they’re only identified when you find several business-capability microservices that need the same technical capability.

NOTE Defining the scope and responsibility of a microservice will be covered in chapter 4.

# INDIVIDUALLY DEPLOYABLE

A microservice should be *individually deployable*. When you change a particular microservice, you should be able to deploy it to the production environment without deploying (or touching) any other part of your system. The other microservices in the system should continue running and working during the deployment of the changed microservice and continue running once the new version is deployed.

Consider an e-commerce site. Whenever a change is made to the shopping cart microservice, you should be able to deploy just that microservice, as illustrated in figure 1.2. Meanwhile, the price calculation, recommendation, and product catalog microservices, and others, should continue working and serving user requests.

![](bbox=[0.18, 0.7, 0.88, 0.75])

Figure 1.2 Other microservices continue to run while the shopping cart microservice is being deployed.

Being able to deploy each microservice individually is important because in a microservice system there are many microservices, and each may collaborate with several others. At the same time, development work is done on some or all of the microservices in parallel. If you had to deploy all or groups of them in lockstep, managing the deployments would quickly become unwieldy, typically resulting in infrequent and big, risky deployments. This is something you should avoid. Instead, you want to be able to deploy small changes to each microservice frequently, resulting in small, low-risk deployments.