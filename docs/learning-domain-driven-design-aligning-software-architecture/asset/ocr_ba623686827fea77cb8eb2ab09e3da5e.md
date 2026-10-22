+   b. Value objects can emit domain events.
+   c. An aggregate contains one or more entities.
+   d. A and C.

# 5. Which of the following statements is correct about differences between active records and aggregates?

+   a. Active records contain only data, whereas aggregates also contain behavior.
+   b. An aggregate encapsulates all of its business logic, but business logic manipulating an active record can be located outside of its boundary.
+   c. Aggregates contain only data, whereas active records contain both data and behavior.
+   d. An aggregate contains a set of active records.

1 Fowler, M. (2002). Patterns of Enterprise Application Architecture. Boston: Addison-Wesley.

2 All the code samples in this chapter will use an object-oriented programming language. However, the discussed concepts are not limited to OOP and are as relevant for the functional programming paradigm.

3 POCOS in .NET, POJOs in Java, POPOs in Python, etc.

4 “Primitive Obsession.” (n.d.) Retrieved June 13, 2021, from https://wiki.c2.com/?PrimitiveObsession.

5 In C# 9.0, the new type record implements value-based equality and thus doesn’t require overriding the equality operators.

6 Also known as a service layer, the part of the system that forwards public API actions to the domain model.

7 In essence, the application layer’s operations implement the transaction script pattern. It has to orchestrate the operation as an atomic transaction. The changes to the whole aggregate either succeed or fail, but never commit a partially updated state.