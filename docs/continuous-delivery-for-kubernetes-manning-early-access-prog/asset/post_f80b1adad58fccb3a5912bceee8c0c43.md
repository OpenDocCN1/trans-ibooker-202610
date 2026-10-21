
Level functions consume `KeyPressedEvents` and produce a `LevelCompletedEvent` when the player finish the level. The `LevelCompletedEvent` is forwarded to the Game Controller which move the game forward.

## Figure 7.14 Linking level functions and the Game Controller using Triggers

In this context, the Game Controller is orchestrating the sequence of levels that the player is going to go through. This orchestration is done without knowing how many levels are available or which level comes next. While this approach is dynamic and can work for simple scenarios, having a level registry can help us to provide a better experience and reduce the complexity for checking which levels are available.

It is important to notice that under high demand, the Broker itself might become the bottleneck as each player will be hitting the same Broker.

## 7.3.2 Dealing with function's State

The game scenario has two main pieces that require us to store state to make sure that the application is resilient to crashes. The Game Controller needs to keep game sessions stored externally to make sure that we can scale this component by having multiple replicas. The information that the Game Controller need to store is not much, but we need to make sure this information is available at all times for the game experience to work as expected. As the Game Controller is keeping where each player is and receiving events when each player completes a level, we want to only keep track of the level statistics as part of moving each session forward. For this reason, the Game Controller will keep track of how much time takes since a player starts a level and the LevelCompletedEvent is received by the level function.

The second place where we will need a way to persist our events is for each function logic, as each level will need to collect events to validate if the player has completed the level or not. Because we have tons of events arriving, we need to make sure that different instances of the levels can access the same received events.

When you have high demand and multiple players sending Events to Level 1, new replicas will be created. If these replicas cannot share the same context, they will not be able to check if the level was completed or not. We need a way to share this state across different replicas.

Figure 7.15 Storing events for aggregation to enable scaling up of our functions

This can be tricky, as latency will play a major role in how reactive our levels are, depending on how much time it takes for us to load all previous events when a new event arrives. We can use a tool like Memcached to store the events when they arrive and then fetch and process all the previous events for a specific player. We can even store snapshots aggregating previous events, but it will all come down to latency and how efficient will be to access external storage for each event that arrives.

Another approach, if you are using a Broker like Kafka, is to use the message broker implementation as a database and aggregate the data using any tool that allow you to consume messages and aggregate them together at that layer. By following this approach you avoid storing the events and aggregating them in different places.

Messages can be aggregated at the infrastructure level and our functions can consume those aggregations. In this case the state is being kept by the message brokers themselves.

Figure 7.16 Using Broker implementation’s tools to do aggregation

![](img/2ab98a5ff1ec073700d902ba95287fd6_199_0.png)

![](img/2ab98a5ff1ec073700d902ba95287fd6_199_1.png)

While this option is more performant, we are now defining application logic (how to aggregate our events) in a Broker’s implementation-specific way.

The last approach that might be worth noting here, is to make functions sticky based on for example the GameSessionId, if we know that the same replica of our level function will receive all the events associated with a `GameSessionId` we can implement a fast local storage mechanism for holding the events as close to the function as possible. Unfortunately, Knative Serving doesn’t support this scenario yet (https://github.com/knative/serving/issues/8160)

## 7.3.3 Monitoring all players and creating a real-time dashboard

As we discussed in the previous two sections, the Game experience is divided into the Game Controller and the events that are being sent to each level function.

We can tap into the Broker events to build a dashboard that shows real-time data about each player interaction. Depending on the game demand, the number of players that we are expecting to be playing concurrently, we can decide to create a Broker per level and by that split the load into different Brokers. From the monitoring/dashboard perspective, we will need to tap into these different brokers to have a full picture of what is going on.

In this chapter we have looked into how, by using a function-based approach, we can provide a simplified experience for our developers building more scoped applications. We have seen how these functions will scale up based on demand and how we can use Knative Eventing to route events to wire functions together to build more complex functionality. We have also seen the challenges that we will face when building this kind of application hence there is always a trade-off. My intention behind discussing a FaaS approach on top of Kubernetes is twofold:

- Promote a polyglot approach that leverage the Kubernetes ecosystem and offers a simplified developer experience by leveraging functions.
- Demonstrate how a developer experience can be built to reduce the cognitive load required to be productive. In this case, `func` aims to remove the complexity of Kubernetes and building containers, but if you are building your own platform you might want to abstract away more aspects to make your developers more productive.

In the next chapter, we will be reusing all we have learned so far in this book to describe all the pieces required to build a platform designed with Continuous Delivery in mind. We will be looking at how such a platform can be built, which tools can be used and what kind of developer experience we should be aiming for if we want to enable our teams to deliver more software more efficiently on top of Kubernetes.

## 7.4 Summary

- A function consumes an event and produce an event as a result. These events in the context of Kubernetes and Cloud-Native applications can be expressed using the CNCF CloudEvents specification.
- You can use Knative `func` to create, build, run locally and deploy a function project into a Kubernetes cluster without writing a Dockerfile or a single line of YAML. These functions can use your preferred programming language, so you can leverage the polyglot nature of Kubernetes.
- You can have one or more functions per container. You can have group functions together if they are all related to each other, for example, they are usually used together. When you group functions together in the same container you will need to define how events arriving to the container are routed to the right function. Two approach discussed in this chapter are `path-based` routing and `attribute-based` routing. Spring Cloud Functions for Spring Boot applications supports both.
- There are a number of Knative and Knative `func` plugins that you can use in VSCode, IntelliJ Idea and GoLand to manage your functions lifecycle.
- You can use Knative eventing resources such as Brokers and Triggers to chain functions together to build more complex interactions.
