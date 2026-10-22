of design patterns. That is why it is so important to know about design patterns and to talk about them.

# A Design Pattern Carries an Intent

By using the name of a design pattern you can express your intent concisely and limit possible misunderstandings. This leads to the second property of a design pattern: an **intent**. The *name* of a design pattern conveys its *intent*. If you use the name of a design pattern, you implicitly state what you consider to be the problem and what you see as a solution.

Hopefully you realized that in our little conversion above we where not talking about any kind of implementation. We did not talk about implementation details, any features, or any particular C++ standard. We did not even talk about any particular programming language. And please do not assume that by giving you the name of a design pattern I have implicitly told you how to implement the solution. That is not what a design pattern is about. On the contrary: the name should tell you about the structure that I propose, about how I plan to manage dependencies and about how I expect the system to evolve. That is the intent.

In fact, many design patterns have a similar structure. In the GoF book, many of the design patterns look very much alike, which, of course, raises a lot of confusion and questions. For instance, structurally there appears to be almost see no difference between the *Strategy*, the *Command*, and the *Bridge* design patterns.⁴ However, their intent is very different and you would therefore use them to solve different problems. As you will see in various examples in the following chapters, there are almost always many different implementations you can choose from.

# A Design Pattern Introduces an Abstraction

A design pattern always provides some way to **reduce dependencies** by introducing some kind of **abstraction**. This means that a design pattern is always concerned with managing the interaction between software entities and decoupling pieces of your software.