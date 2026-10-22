
```
    return EXIT_SUCCESS;
}
```

Since up to C++17, the Database class counts as an aggregate type, which means that value initialization would be performed via aggregate initialization. Aggregate initialization, in turn, would ignore the default constructor, including the fact that it is private, and simply perform a zero initialization of the object. Thus value initialization enables you to still create an instance. If, however, you provide the default constructor yourself, then the class does not count as aggregate type, which prevents aggregate initialization.⁵

The instance() function is implemented in terms of a static local variable. This means, that the first time control passes through the declaration, the variable is initialized in a thread-safe way, and on all further calls the initialization is skipped.⁶ On every call, the first, but also all subsequent calls, the function returns a reference to the static local variable.

The rest of the Database class is pretty much what you would expect from a class representing a database: there are some public, database-related functions (e.g. write() and read()) and there could be some data members, including access functions. In other words, except for the instance() member function and the special members, Database is just a normal class.

## Singleton Does Not Manage Or Reduce Dependencies

Now, with one possible implementation of a Singleton in mind, let us go back to my claim that Singleton is not a design pattern. First, let us remind ourselves of the properties of a design pattern, which I have defined in “Guideline 11: Understand the Purpose of Design Patterns”:

A design pattern ...

- ... has a name;
- ... carries an intent;
- ... introduces an abstraction;
- ... has been proven.

The *Singleton* pattern definitely has a name, and it definitely has an intent. No question there. I would also claim, that it has been proven over the years (although there may be sceptical voices, that point out that *Singleton* is rather infamous). However, there is no kind of abstraction around: no base class, no template parameters, nothing. *Singleton* does not represent an abstraction itself and it does not introduce an abstraction. In fact, it is not concerned with the structure of code, with the interaction and interdependencies of entities, and hence it is not aiming at managing or reducing dependencies.⁷ This, though, is what I defined to be an integral part of software design. Instead, *Singleton* is focusing on restricting the number of instantiations to exactly one. Thus, as a consequence, *Singleton* is not a design pattern, but merely an implementation pattern.

“Then why is it listed as a design pattern in so many important sources?” you are asking. This is a fair and good question. There may be three answers to that: first, in other programming languages, in particular languages where every class can automatically represent an abstraction, the situation may be different. While I acknowledge this, I still believe that the intent of the *Singleton* pattern is primarily targeted for implementation details and not for dependencies and decoupling.

Second, *Singleton* is very commonly used (although often also *misused*), so it is definitely a pattern. Since there are *Singletons* in many different programming languages, it does not appear to be just an idiom of the C++ programming language. As a consequence, it appears reasonable to call it a design pattern. This chain of arguments may sound plausible to you, but I feel it falls short of distinguishing between software design and implementation details. This is why in “Guideline 11: Understand the Purpose of Design Patterns”, I have introduced the term *implementation pattern* to be able to distinguish between different kinds of language-agnostic patterns such as *Singleton*.⁸

And third, I believe that we still are in the process of understanding software design and design patterns. There is no common definition of software design. For that reason, I came up with one in “Guideline 1: Understand the Importance of Software Design”. There is no common definition of design patterns, either. This is why I came up with one in “Guideline 11: Understand the Purpose of Design Patterns”. I strongly believe that we must talk more about software design and more about patterns to come to a common understanding of the necessary terminology, especially in C++.

In summary, you do not use a Singleton to decouple software entities. So despite the fact, that it is described in the famous GoF book, in “Code Complete” or even listed as a design pattern on Wikipedia, it does not serve the purpose of a design pattern. Singleton is merely dealing with implementation details, and as such you should treat it as an implementation pattern.

## GUIDELINE 37: TREAT SINGLETON AN IMPLEMENTATION PATTERN, NOT A DESIGN PATTERN

- The goal of Singleton is not to decouple or manage dependencies and thus it does not fulfill the expectations of a design pattern;
- Apply the Singleton pattern with the intent to restrict the number of instances of a particular class to exactly one.

## Guideline 38: Design Singletons for Change and Testability

Singleton is indeed a rather infamous pattern: There are many voices out there, that describe Singleton as a general problem in code, as an anti-pattern, as dangerous, or even as evil. Therefore, there is a lot of advice out there to avoid the pattern, amongst others Core Guideline I.3.:⁹

Avoid singletons.

—Core Guideline I.3

One of the primary reasons, why people dislike Singleton is, that it often causes artificial dependencies and obstructs testability. As such, it runs contrary to two of the most important and most general guidelines in this book: “Guideline 2: Design for Change” and “Guideline 4: Design for Testability”. From that perspective, Singleton indeed appears to be a problem in code and should be avoided. However, despite all the good intentioned warnings, the pattern is persistently used by many developers. The reasons for that are manifold, but probably mainly related to two facts: first, sometimes (and let us agree on sometimes) it is desirable to express the fact, that something exists only once and should be available for many entities in the code. Second, sometimes Singleton appears to be the proper solution, as there are global aspects to represent.

So, let us do the following: Instead of arguing that Singleton is always bad and evil, let us focus on those few situations, where we need to represent a global aspect in our program, and discuss how to represent this aspect properly, but how still to design for change and testability.

## Singletons Represent Global State

Singletons are mostly used to represent entities in a program, that logically and/or physically exist only once and that should be used by many other classes and functions. 10 Common examples are the system-wide database, logger, clock, or configuration. These examples, including the term “system-wide”, give an indication of the nature of these entities: they commonly represent globally available functionality or data, i.e. global state. From that perspective, the Singleton pattern appears to make sense: by preventing everyone from creating new instances, by forcing everyone to use the one instance, you can guarantee uniform and consistent access to this global state across all using entities.

This representation and introduction of global state, however, explains the reason, why Singleton is commonly considered a problem. As Michael

Feathers expressed it:¹¹

> *The singleton pattern is one of the mechanisms people use to make global variables. In general, global variables are a bad idea for a couple of reasons. One of them is opacity.*
> 
> —Michael Feathers

Global variables are indeed a bad idea, particularly for one important reason: the term “variable” suggests, that we are talking about *mutable* global state. And that kind of state can indeed cause a lot of headache. To be explicit, mutable global state is frowned upon (in general, but especially in a multithreaded environment), as it is difficult, costly, and likely both, to control access and to guarantee correctness. Furthermore, global (mutable) state is very hard to reason about, as read and write access to this state usually happens invisibly within some function, which, based on its interface, does not reveal the fact that it uses the global state. And last but not least, if you have several globals, whose lifetime depends on each other and that are distributed over several compilation units, you might be facing the static initialization order fiasco (or short *SIOF*).¹² So obviously, it is beneficial to avoid global state as much as possible.¹³

The problem of global state, however, is a problem that we cannot resolve by avoiding *Singletons*. It is a general problem, unrelated to any particular pattern. The same problem, for instance, also exists for the *Monostate* pattern, which enforces a single, global state, but allows for any number of instantiations.¹⁴ So on the contrary, *Singleton* can help dealing with the global state by constraining access to it. For instance, as Miško Hevery explains in his 2008 article, *Singletons* that provide a unidirectional data flow to *or* from some global state, are acceptable:¹⁵ a *Singleton* implementing a logger would only allow you to write data, but not to read it. A *Singleton* representing a system-wide configuration or clock would only allow you to read the data, but not to write it, thus representing a global *constant*. The restriction to unidirectional data flow helps to avoid many of the usual problems with global state. Or in the words of Miško Hevery (the emphasis being mine):
