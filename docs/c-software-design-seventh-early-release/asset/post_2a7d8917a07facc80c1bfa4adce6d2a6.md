
wrapper, which injects the globally set `PersistenceInterface`. In this combination, it is possible to make local decisions and to locally inject the desired dependency, but at the same time it is not necessary to pass the dependency through many layers of function calls.

However, truth be told, while these solutions may work very well in this database example and also in the context of managing memory, it might not be the right approach for every single *Singleton* problem. So do not believe this is the only possible solution. After all, it depends. However, it is a great example for the general process of software design: identify the aspect that changes or causes dependencies, then separate concerns by extracting a fitting abstraction. Depending on your intent, you will just have applied a design pattern. So consider naming your solution accordingly and by that leave traces of your reasoning for others to pick up on.

In summary: the *Singleton* pattern certainly is not one of the glamorous patterns. It simply comes with too many disadvantages, most importantly the usual flaws of global state. But still, despite the many negative aspects, if used judiciously, *Singleton* can be the right solution for representing the few global aspects in your code in some situations. If it is, prefer *Singletons* with unidirectional data flow and design your *Singletons* for change and testability by inverting the dependencies and enabling *dependency injection* with the *Strategy* design pattern.

## GUIDELINE 38: DESIGN *SINGLETONS* FOR CHANGE AND TESTABILITY

- Be aware that *Singleton* represents global state, with all its flaws;
- Avoid global state as much as possible;
- Use *Singleton* judiciously and just for the few global aspects in our code;
- Prefer *Singletons* with unidirectional data flow;
- Use the *Strategy* design pattern to invert dependencies on your *Singleton* to remove the usual impediment to changeability and testability.

1 Steve McConnell: “Code Complete: A Practical Handbook of Software Construction”, 2nd Edition, Microsoft Press, Jul. 2004, ISBN: 978-0735619678.

2 “There can be only one” is the tagline of the 1986 movie “Highlander” featuring Christopher Lambert (Wikipedia).

3 The Meyers’ Singleton is explained in Item 4 of Scott Meyers’ “Effective C++, Third Edition: 55 Specific Ways to Improve Your Programs and Design”, Addison-Wesley, May 2005, ISBN: 978-0321334879.

4 I know that the explicit handling of the copy and move assignment operators appear to be overkill, but this gives me the opportunity to remind you about the Rule of 5.

5 This behavior has changed in C++20, since the declaration of any constructor by the user is now enough to make a type non-aggregate.

6 To be precise and to avoid complaints: in case the static local variable is zero or constant initialized, the initialization can happen before the function is entered. In our example, the variable is indeed created in the first pass.

7 In fact, a naive implementation of Singleton is creating lots of artificial dependencies itself; see “Guideline 38: Design Singletons for Change and Testability”

8 Without going into detail, I argue that there are several more so called “design patterns”, that fall in the category of implementation patterns, for instance the Monostate pattern, the Memento pattern, or also the RAII idiom, which Wikipedia lists as design pattern. While this

might make sense in languages other than C++, the intent of RAII is most certainly not to reduce dependencies, but to automate cleanup and to encapsulate responsibility.

9 Another such piece of advice is the CppCon 2020 talk by Peter Muldoon, “Retiring the Singleton Pattern: Concrete Suggestions for What to use Instead” (Youtube), which provides many useful techniques for how to deal with Singletons in your codebase.

10 If a Singleton is used for anything else, you should be very suspicious and consider it a misuse of the Singleton pattern.

11 Michael Feathers: “Working Effectively with Legacy Code”, Addison-Wesley, 14th Edition, Jan. 2013, ISBN: 978-0131177055.

12 The best summary of SIOF I am aware of is given by Jonathan Müller in his accordingly named Meeting C++ 2020 talk (YouTube).

13 “Globals are bad, m’kay?”, as stated by Guy Davidson and Kate Gregory in “Beautiful C++: 30 Core Guidelines for Writing Clean, Safe, and Fast Code”, Addison Wesley, Dec. 2021, ISBN: 978-0137647842.

14 The Monostate pattern, to my best knowledge was first mentioned in the September issue of the 1996 C++ report in the article “Monostate Classes: The Power of One” by Steve Ball and John Crawford (see “More C++ Gems”, Cambridge University Press, Feb. 2000, ISBN: 978-0521786188). It is also described in Martin Reddy’s “API Design for C++”, Morgan Kaufmann, Feb. 2011, ISBN: 978-0123850034. Monostate, in constrast to Singleton, allows any number of instances of a type, but makes sure that there is only a single state for all instances. As such, the pattern should not be confused with std::monostate, which is used as a well-behaved empty alternative in std::variant.

15 Miško Hevery, “Root Cause of Singletons”, Aug. 2008, Blog.

16 For an explanation about the different kinds of test doubles, see Martin Fowler’s article “Mocks Aren’t Stubs” (martinfowler.com). For examples on how to use these in C++, refer to Jeff Langr’s “Modern C++ Programming with Test-Driven Development”, O’Reilly, Oct. 2013, ISBN: 978-1937785482.

17 But I am sure you will not be deterred from writing the tests anyway, despite it being difficult.

18 This is also one of the strong arguments in Robert C. Martin’s “Clean Architecture”, Addison-Wesley, Sep. 2017, ISBN: 978-0134494166.

19 For the design patterns experts I should explicitly point out, that the std::pmr::get_default_resource() function itself fulfills the intent of another design pattern: the Facade design pattern. Unfortunately, I do not go into detail about Facade in this book.

20 The separation into a public interface and a private implementation is an example for the Template Method design pattern. Unfortunately, in this book I cannot go into detail about the many benefits of this design pattern.

## Chapter 11. The Last Guideline

## A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 11th chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at sevans@oreilly.com.

There is only one more guideline, one more piece of advice that I can bestow upon you. So here is it: the last guideline.

## Guideline 39: Continue to Learn About Design Patterns

“That’s it? This is all you got? Come on, there are so many more design patterns out there. We barely touched the surface!”, you complain. Well, honestly, you are completely correct, there is nothing I can add to that. But in my defense, I was planning for many more patterns, until reality struck me: there is just so much information that you can fit into a book with 400 pages. But do not fret: in these 400 pages I have taken you on a journey through the most important pieces of advice for any design that you will need anywhere, anytime in your software development career:

- Minimize dependencies: dealing with dependencies is the core of software design. And whatever kind of software you write, if you are seriously interested to make it last, you will have to deal with dependencies: the necessary ones, but primarily the artificial ones. Of course your major goal is to reduce dependencies, hopefully even minimize dependencies. And to achieve this goal, you will inevitably deal with design patterns.
- Separate concerns: this may be the most important, the central design guideline that you can take away from this book. Separate concerns and your software structures will detangle, will become easier to understand, easier to change, and easier to test. All design patterns, without exception, provide you with some way to separate concerns. The major difference between different patterns is the way they separate concerns, their *intent*. Although design patterns may be structurally similar, their intent is always unique.
- Prefer composition to inheritance: while inheritance is a powerful feature, the true strength of many design patterns stems from building on composition. For instance, the *Strategy* design pattern, one of the patterns that is used *everywhere* (and hopefully this has become obvious by now), primarily builds on composition to separate concerns, but then also offers you the option to use inheritance to extend the functionality. The same is true for *Bridge*, *Adapter*, *Decorator*, *External Polymorphism*, and *Type Erasure*.
- Prefer a non-intrusive design: true flexibility and extendability arise when it is not necessary to modify existing code, but possible to just add new code. Therefore any design that is non-intrusive is preferable to design that intrusively modifies existing code. Hence design patterns such as *Decorator*, *Adapter*, *External Polymorphism* and *Type Erasure* are such valuable additions to your design pattern toolbox.
- Prefer *value semantics* over *reference semantics*: in order to keep code simple, understandable, and away from dark corners such as nullptrs, dangling pointers, lifetime dependencies, etc., you should prefer to
