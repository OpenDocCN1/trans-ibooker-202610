# 1.7.2 Side effects of the user request

At this particular e-commerce website, when a user adds an item to their shopping cart, a couple of actions happen in addition to adding the item to the cart:

+   1. The recommendation engine updates its internal model to reflect the fact that the user has shown a high degree of interest in that particular product.
+   2. The tracking service records that the user added the item to their cart in the tracking database. This information may be used later for reporting or other business intelligence purposes.

Neither of these actions needs to happen in the context of the user’s request; they may as well happen after the request has ended, when the user has received a response and is no longer waiting for the backend system.

You can think of these types of actions as side effects of the user’s request. During the request the fact that the user added an item to the shopping is recorded as an event. The side effects aren’t direct effects of the request to update the user’s shopping cart; they’re secondary effects that happen as reactions to the event that recorded that the item was added to the cart. Figure 1.7 zooms in on the side effects of adding an item to the cart.

![](bbox=[0.175, 0.475, 0.795, 0.705])

Figure 1.7 The shopping cart microservice publishes events, and other subscribing microservices react.

The trigger for these side effects is an ItemAddedToCart event published by the Shopping Cart microservice. Two other microservices subscribe to events from shopping cart and take the necessary actions as events (such as ItemAddedToCart events) occur. These two subscribers react to the events asynchronously—outside the context of the original request—so the side effects may happen in parallel with the main handling of the request or after the main handling has completed.