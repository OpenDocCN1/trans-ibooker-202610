In this chapter, I'll show you how to implement all three collaboration styles in code using HTTP GET, POST, PUT, and DELETE endpoints. I will also show how to deploy several collaborating microservices to Kubernetes and have them collaborate as intended using all three collaboration styles.

# 5.1 Types of collaboration: Commands, queries, and events

Microservices are fine grained and narrowly scoped. To deliver functionality to an end user, microservices need to collaborate.

As an example, consider the loyalty program microservice from the POS system in chapter 4. The loyalty program microservice is responsible for the loyalty program business capability. The program is simple: customers can register as users with the loyalty program; once registered, they receive notifications about new special offers and earn loyalty points when they purchase something. Still, the loyalty program business capability depends on other business capabilities, and other business capabilities depend on it. As illustrated in figure 5.1, the loyalty program microservice needs to collaborate with a number of other microservices.

![](bbox=[0.17, 0.45, 0.88, 0.68])

Figure 5.1 The loyalty program microservice collaborates with several other microservices. In some cases, the loyalty program microservice receives requests from other microservices; at other times, it sends requests to other microservices.

As stated in the list of microservice characteristics in chapter 1, a microservice is responsible for a single capability; and as discussed in chapter 4, that single capability is typically a business capability. End-user functionalities—or use cases—often involve several business capabilities, so the microservices implementing these capabilities must collaborate to deliver functionality to the end user.