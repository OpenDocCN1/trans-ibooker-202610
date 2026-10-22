Meyers’ Singleton. I will, however, also make a strong argument to not treat Singleton as a design pattern, but an implementation pattern.

In “Guideline 38: Design Singletons for Change and Testability”, we accept the fact that sometimes we need a solution to represent the few global aspects in your code. This is what the Singleton pattern is often used for.

This also means that we are confronted by the usual problems of Singletons: global state, many strong, artificial dependencies, and an impeded changeability and testability. While this sounds like an excellent reason to avoid Singleton after all, I will show you that by proper software design, you can combine the Singleton benefits with excellent changeability and testability.

# Guideline 37: Treat Singleton as an Implementation Pattern, Not a Design Pattern

Let me start by addressing the elephant in the room:

Singleton is not a design pattern.

If you have not heard about Singleton before, then this might not make any sense at all, but bear with me, I promise to explain Singleton shortly. If you have heard about Singleton before, then I assume your reaction being one of two things: Either you are nodding in agreement, putting up this sympathizing “I know” look, or you are utterly stunned and initially do not know what to say. “But why not”, you eventually dare to ask, “isn’t it one of the original design patterns from the Gang-of-Four (GoF) book?” Yes, you are correct: Singleton is one of the 23 original patterns documented in the GoF book. At the time of writing Wikipedia calls it a design pattern and it is even listed as a design pattern in Steve McConnell’s bestseller “Code Complete”.¹ Nevertheless, it is still not a design pattern, because it does not have the properties of a design pattern. Let me explain.

# The Singleton Pattern Explained