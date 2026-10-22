In “Guideline 33: Know about the Optimization Potential of Type Erasure”, I will make an exception: despite the fact that in this book I primarily focus on dependencies and design aspects, in this one guideline I will entirely focus on performance-related implementation details. I will show you how to apply the *Small Buffer Optimization (SBO)* and how to implement a manual virtual dispatch to speed up your *Type Erasure* implementation.

In “Guideline 34: Be Aware of the Setup Costs of Owning Type Erasure Wrappers”, we will investigate the setup costs of the owning *Type Erasure* implementation. We will find, that there is a cost associated to *value semantics*, that sometimes we are not willing to pay. For this reason, we dare to take a step into the realm of *reference semantics* and implement a form of non-owning *Type Erasure*.

# Guideline 32: Consider Replacing Inheritance Hierarchies with Type Erasure

There are a couple of recurring pieces of advice throughout this book:

+   - Minimize dependencies.
+   - Separate concerns.
+   - Prefer composition to inheritance.
+   - Prefer non-intrusive solutions.
+   - Prefer *value semantics* over *reference semantics*.

Used on their own, all of these have very positive effects on the quality of your code. In combination, however, these guidelines prove to be so much better. This is what you have experienced in our discussion about the *External Polymorphism* design pattern in “Guideline 31: Use External Polymorphism for Non-Intrusive Runtime Polymorphism”. Extracting the polymorphic behavior turned out to be extremely powerful and has unlocked an unprecedented level of loose coupling. Still, probably disappointingly, the demonstrated implementation of *External*