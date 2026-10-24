You can count if you do not believe it, but there are exactly 10 lines reporting the `fc71afb7-604d-4cbd-9b20-0c604565ed9a` **globally unique identifier (GUID)**, which corresponds to our thread-safe implementation. For the non-thread-safe implementation, we count two different GUIDs: one starting with `60fe4df0` and another one starting with `f6189dd1`. This means that we indeed got two different instances out of our poor implementation.

# Singleton pattern wrap-up

The singleton pattern comes with the following benefits and drawbacks:

+   - Enhanced performance, especially for slow-initializing and heavy objects, which is a valid use case.
+   - Reduced usage of memory, because a single instance will of course consume less memory than multiple instances of the same object.
+   - Thread safety is not guaranteed by default, as we saw in the previous section.
+   - It can be harder to react to ecosystem changes such as underlying resource exhaustion, security token expiration, loss of network connectivity, and so on.
+   - It violates the SRP because it deals with both its own instantiation and some business logic.

Use the singleton pattern with caution and only when required. Valid use cases include cross-cutting concerns such as loggers, **object-relational models (ORMs)** when they remain thread-safe, third-party factories, and so on. Remember that singleton drawbacks are as important as their benefits and you should always double-check the concurrency aspects, as well as how you can detect/react to ecosystem changes. When working with rich frameworks such as .NET, you can also rely on the built-in DI container to assign a singleton behavior to a class that is not itself developed as such, while ensuring that only one single instance will ever be created.

# Factory method

The factory method pattern, part of the creational category, is also part of a larger factory family of patterns such as **static factory** and **abstract factory**. Factory patterns, in general, are somewhat similar in their purpose to DI: decoupling concrete classes from client consumer classes. There is sometimes a bit of confusion between DI and factory patterns, precisely because they are both creational and pursue the same goals.