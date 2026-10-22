# GUIDELINE 14: USE A DESIGN PATTERN’S NAME TO COMMUNICATE INTENT

+   - Use the name of a design pattern to communicate the intent of a solution;
+   - Use the name of a design pattern to improve readability.

1 The Gang of Four, or simply GoF, is a commonly used reference to the four authors Erich Gamma, Richard Helm, Ralph E. Johnson, and John Vlissides and their book on design patterns: “Design Patterns, Elements of Reusable Object-Oriented Software”, Prentice Hall, Oct. 1994, ISBN: 978-0201633610. The GoF book still is, after several decades, the reference on design patterns. Throughout the rest of this book, I will refer to either the GoF book, the GoF patterns, or the characteristic, object-oriented GoF style.

2 If you do not know the Visitor design pattern yet, do not worry. I will introduce the pattern in Chapter 4.

3 The Strategy design pattern will be explained in detail in Chapter 5, the Decorator design pattern in Chapter 9.

4 I only mention the design patterns that I will explain in later chapters (see the Strategy and Command design pattern in Chapter 5, and the Bridge design pattern in “Guideline 28: Build Bridges to Remove Physical Dependencies”). There are a few more design patterns that share the same structure.

5 In case you are unfamiliar with the Strategy design pattern, rest assured that Chapter 5 will provide much more information, including several code examples.

6 This may be a controversial example. Since I know the C++ community, I know that you may have a different opinion. However, I stand by mine: due to its definition, std::make_unique() is incapable of decoupling software entities and therefore does not play a role on the level of software design. It is merely an implementation detail (but a valuable and useful one).

7 Venkat Subramaniam and Andrew Hunt: “Practices of an Agile Developer”, The Pragmatic Programmers, LLC, July 2017, ISBN: 978-0974514086.

8 Well, it works, in some definition of “works”.

9 I know what you are thinking: “You cannot be serious! There is so many interesting examples out there, but you select the oldest and most boring example in the book!”. Ok, I admit, that might not be the most exciting example to pick. But, still, I have two good reasons to use this example. First, the scenario is so well known, that I can assume that no-one has trouble understanding it. That means that everyone should be able to follow my arguments about