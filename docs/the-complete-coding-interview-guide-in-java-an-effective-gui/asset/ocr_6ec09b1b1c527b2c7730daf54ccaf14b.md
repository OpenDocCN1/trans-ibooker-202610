# Can we write virtual methods in Java?

Yes, we can! Actually, in Java, all non-static methods are, by default, virtual methods. We can write a non-virtual method by marking it with the `private` and/or `final` keyword. In other words, the methods that can be inherited for polymorphic behavior are virtual methods. Or, if we turn the logic of this statement on its head, the methods that cannot be inherited (marked as `private`) and the methods that cannot be overridden (marked as `final`) are non-virtual.

# What is the difference between polymorphism and abstraction?

Abstraction and polymorphism represent two fundamental OOP concepts that are interdependent. Abstraction allows the developer to design general solutions that are reusable and customizable, while polymorphism allows the developer to defer choosing the code that should be executed at runtime. While abstraction is implemented via interfaces and abstract classes, polymorphism relies on overriding and overloading techniques.

# Do you consider overloading an approach for implementing polymorphism?

This is a controversial topic. Some people do not regard overloading as polymorphism; therefore, they do not accept the idea of compile-time polymorphism. Such voices maintain that the only overriding method is genuine polymorphism. The argument behind this statement says that only overriding allows code to behave differently depending on the runtime conditions. In other words, exhibiting polymorphic behavior is the privilege of method overriding. I consider that as long as we understand the premises of overloading and overriding, we also understand how both variants sustain polymorphic behavior.

**Important note**
Questions that tackle controversial topics are delicate and hard to approach correctly. Therefore, it is advisable to jump into the answer directly with this statement *This is a controversial topic.* Of course, the interviewer is interested to hear your opinion as well, but he will be pleased to see that you know both sides of the coin. As a rule of thumb, try to answer in an objective manner and don't approach one side of a coin with radicalism or with a poor arsenal of arguments. Controversial things remain controversial after all, and this is not the proper time and place to demystify them.