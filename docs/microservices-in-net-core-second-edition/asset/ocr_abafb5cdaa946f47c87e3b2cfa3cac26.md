The shopping cart microservice supports three types of synchronous requests:

+   - Getting a shopping cart
+   - Adding an item to a shopping cart
+   - Deleting an item from a shopping cart

On top of that, it exposes an event feed that other microservices can subscribe to. Now that you’ve seen an overview of the shopping cart microservice’s complete functionality, you can start drilling into its implementation.

# 2.1.1 Components of the Shopping Cart microservice

Let’s zoom in and see what this microservice looks like at closer range. As shown in figure 2.3, the shopping cart microservice consists of these components:

+   - A small component called shopping cart domain model responsible for implementing any business rules related to shopping carts.
+   - An HTTP API component that’s responsible for handling all incoming HTTP requests. The HTTP API component is divided into two parts: one handles requests from other microservices to do something, and the other exposes an event feed.

![](bbox=[0.11, 0.47, 0.9, 0.7])

Figure 2.3 The shopping cart microservice is a small codebase with a few components that provide one focused business capability.

There are two data store components: EventStore and ShoppingCartStore. These data store components are responsible for talking to the data store (Shopping Cart Database):

+   - EventStore handles saving events to and reading them from the data store.
+   - ShoppingCartStore handles reading and updating shopping carts in the data store. Note that shopping carts and events may be stored in different databases; we’ll return to this in chapter 6.