3 Whether or not the code modification is risky may very much depend on your test coverage. Thus a good test coverage may actually absorb some of the damage bad software design may cause.

4 Kent Beck: “Test-Driven Development By Example”, Addison-Wesley, Nov. 2002, ISBN: 978-0321146533.

5 Robert C. Martin: “Clean Architecture”, Addison-Wesley, Sep. 2017, ISBN: 978-0134494166.

6 Just to be clear: computer science is a science (it is in the name). Software engineering appears to be a hybrid form of science, craft, and art. And one aspect of the latter is software design.

7 With this metaphor I do not try to give the impression that architects for buildings work at the construction site all day. Very likely, such an architect spends more time in a comfy chair and in front of a computer than people like you and me. But I think you get the point.

8 Do not worry if you are not aware of SFINAE. Substitution Failure Is Not An Error, or short SFINAE, is a basic template mechanism, which is commonly used as a substitute for C++20 concepts to constrain templates; For an explanation of SFINAE and in particular std::enable_if, refer to your favorite textbook about C++ templates. If you do not have any, a great choice is the C++ template bible: David Vandevoorde, Nicolai Josuttis, Douglas Gregor: “C++ Templates: The Complete Guide”, Addison Wesley, Dec. 2017, ISBN: 978-0321714121.

9 For a lot more information on physical and logical dependency management, see John Lakos’ “dam” book: John S. Lakos: “Large-Scale C++ Software Development: Process and Architecture”, Addison-Wesley Longman, July 2018, ISBN: 978-0201717068.

10 Martin Fowler, “Who Needs an Architect?”; IEEE Software, Volume 20, Issue 5, Sep. 2003, pp 11-13, DOI.

11 A very good introduction to Microservices can be found in Sam Newman’s book “Building Microservices: Designing Fine-Grained Systems (2nd Edition)”, O’Reilly, September 2021, ISBN: 978-1492034025.

12 Mark Richards, Neal Ford: “Fundamentals of Software Architecture: An Engineering Approach”, O’Reilly UK Ltd, February 2020, ISBN: 978-1492043454.

13 The term implementation pattern was first used in Kent Beck’s book “Implementation Patterns”, Addison-Wesley, Oct. 2007, ISBN: 978-0321413093. In this book, I am using that term to provide a clear distinction from the term design pattern, since the term idiom may refer to a pattern on either the level of Software Design or the level of Implementation Details. I will use the term consistently to refer to commonly used solutions on the level of Implementation Details.

14 Second favorite after this one, of course. If this is your only book, then you might refer to the classic “Effective C++ - Third Edition” by Scott Meyers, Addison-Wesley, May 2005, ISBN: 978-0321334879.

15 The Template Method and Bridge design patterns are two of the 23 classic design patterns introduced in the so-called Gang of Four (GoF) book: Erich Gamma, Richard Helm, Ralph E.