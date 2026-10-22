+   - Subscription to events in a message broker
+   - Message topics for publishing outgoing events

All of these are the means for the system to receive requests from the external environment and communicate the output. Strictly speaking, the presentation layer is the program’s public interface.

![](bbox=[0.13, 0.25, 0.86, 0.33])

Figure 8-2. Presentation layer

# Business Logic Layer

As the name suggests, this layer is responsible for implementing and encapsulating the program’s business logic. This is the place where business decisions are implemented. As Eric Evans says,¹ this layer is the heart of software.

This layer is where the business logic patterns described in Chapters 5–7 are implemented—for example, active records or a domain model (see Figure 8-3).

![](bbox=[0.13, 0.6, 0.86, 0.68])

Figure 8-3. Business logic layer

# Data Access Layer

The data access layer provides access to persistence mechanisms. In the pattern’s original form, this referred to the system’s database. However, as in the case of the presentation layer, the layer’s responsibility is broader for modern systems.