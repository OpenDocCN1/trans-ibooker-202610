

incorrectly. While I cannot help with the first point (I am just a mere mortal), I will go into detail about the second point at the end of this guideline.

5 In case you are wondering about the incomplete implementation: the focus here is entirely on how to *design* allocators, not on how to *implement* an allocator. For a thorough introduction on how to implement a C++17 allocator, see Nicolai Josuttis’ “C++17 - The Complete Guide”, NicoJosuttis, Sept. 2019, ISBN: 978-3967309171.

6 The metaphor of *Strategy* being the guts of an object and *Decorator* being the skin originates from the GoF book.

7 Scott Meyers, “Effective C++ - Third Edition”, Addison-Wesley, May 2005, ISBN: 978-0321334879.

8 If right now you are thinking that the original `price()` function should be renamed to `netPrice()` to reflect its true purpose, then I agree.

9 Note, that it is only possible to use floating-point values as non-type template parameters (NTTPs) since C++20. Alternatively, you could store the discount and tax rates in form of data members.

10 Alternatively, in particular if you cannot use C++20 concepts yet, this is an opportunity to use the *Curiously Recurring Template Pattern (CRTP)*; see “Guideline 26: Use CRTP to Introduce Static Type Categories”.

11 In order to avoid a visit from the tax collection office, I should explicitly state that I am aware of the questionable nature of the `Discounted<0.2, Taxed<0.19, ConferenceTicket>>` class (see also the list of potential problems of *Decorator* at the end of “Guideline 35: Use Decorators to Add Customization Hierarchically”). In my defense: it is an obvious permutation of decorators, which is well suited for this benchmark.

12 For a thorough overview of *Type Erasure*, see Chapter 8 and in particular “Guideline 32: Consider Replacing Inheritance Hierarchies with Type Erasure”.

## Chapter 10. The Singleton Pattern

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 10th chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at sevans@oreilly.com.

In this chapter, we take a look at the (in-)famous *Singleton* pattern. I know, you may already be acquainted with *Singleton* and you may already have a strong opinion about that pattern. It is even possible that you consider *Singleton* an anti-pattern and thus ask yourself how I muster the courage to include it in this book. Well, I am aware that *Singleton* is not particular popular and in many circles has a rather bad reputation, in particular because of the global nature of *Singletons*. From that perspective, however, it might be very surprising to learn, that there is a couple of "Singleton"-like instances in the C++ Standard Library. Seriously! And, honestly, they work fantastically! Therefore we should seriously talk about *what* a *Singleton* is, about *when* *Singleton* works and about *how* to deal with *Singleton* properly.

In “Guideline 37: Treat Singleton as an Implementation Pattern, Not a Design Pattern”, I will explain the *Singleton* pattern and demonstrate how it works by means of a very commonly used implementation, the so called

Meyers’ Singleton. I will, however, also make a strong argument to not treat Singleton as a design pattern, but an implementation pattern.

In “Guideline 38: Design Singletons for Change and Testability”, we accept the fact that sometimes we need a solution to represent the few global aspects in your code. This is what the Singleton pattern is often used for.

This also means that we are confronted by the usual problems of Singletons: global state, many strong, artificial dependencies, and an impeded changeability and testability. While this sounds like an excellent reason to avoid Singleton after all, I will show you that by proper software design, you can combine the Singleton benefits with excellent changeability and testability.

### Guideline 37: Treat Singleton as an Implementation Pattern, Not a Design Pattern

Let me start by addressing the elephant in the room:

Singleton is not a design pattern.

If you have not heard about Singleton before, then this might not make any sense at all, but bear with me, I promise to explain Singleton shortly. If you have heard about Singleton before, then I assume your reaction being one of two things: Either you are nodding in agreement, putting up this sympathizing “I know” look, or you are utterly stunned and initially do not know what to say. “But why not”, you eventually dare to ask, “isn’t it one of the original design patterns from the Gang-of-Four (GoF) book?” Yes, you are correct: Singleton is one of the 23 original patterns documented in the GoF book. At the time of writing Wikipedia calls it a design pattern and it is even listed as a design pattern in Steve McConnell’s bestseller “Code Complete”.¹ Nevertheless, it is still not a design pattern, because it does not have the properties of a design pattern. Let me explain.

### The Singleton Pattern Explained

Sometimes you may want to guarantee that there is only one, and *exactly* one, instance of a particular class. In other words, you have a Highlander situation: “There can be only one”.² This might make sense for the system-wide database, the one and only logger, the system clock, the system configuration, or, in short, any class that should not be instantiated multiple times, since it represents something, that exists only once. That is the intent of the *Singleton* pattern:

### THE *SINGLETON* PATTERN

Intent: “Ensure a class has only one instance, and provide a global point of access to it.” (GoF)

This intent is visualized by the Gang of Four with the UML diagram in Figure 10-1, which introduces the `instance()` function as the global point of access to the unique instance:

### Singleton

static instance()
singletonOperation()
getSingletonData()

static uniqueInstance
singletonData

return uniqueInstance

Provides access to the one instance of the Singleton

Figure 10-1. The UML representation of the Singleton pattern.

There are multiple ways to restrict the number of instantiations to exactly one. Definitely one of the most useful and therefore most commonly used forms of Singleton is the Meyers’ Singleton.³ The following Database class is implemented as a Meyers’ Singleton:

```
//---- <Database.h> ----------------

class Database final
{
public:
    static Database& instance() ①
    {
        static Database db;  // The one, unique instance
        return db;
    }

    bool write( /*some arguments*/ );
    bool read( /*some arguments*/ ) const;
    // ... More database-specific functionality

    // ... Potentially access to data members

private:
    Database() { } ②
    Database( Database const& ) = delete;
    Database& operator=( Database const& ) = delete;
    Database( Database&& ) = delete;
    Database& operator=( Database&& ) = delete;

    // ... Potentially some data members
};
```

The Meyers’ Singleton evolves around the fact, that it is only possible to access the single instance of the Database class via the public, static instance() function ①:

```
#include <Database.h>
#include <cstdlib>

int main()
```
```
{    // First access, database object is created    Database& db1 = Database::instance();    // ...    // Second access, returns a reference to the same object    Database& db2 = Database::instance();    assert( &db1 == &db2 );    return EXIT_SUCCESS;}
```

Indeed, this function is the only way to get a Database: all functionality that could possibly be used to create, copy or move an instance, is either declared in the private section, or is explicitly deleted.⁴ Although this appears to be pretty straightforward, one implementation detail is of special interest: note, that the default constructor is explicitly defined and not defaulted ②. The reason is, that in case it was defaulted, up to C++17, it would be possible to create a Database by means of an empty set of braces, i.e. via value initialization:

```
#include <cstdlib>

class Database
{
public:
    // ... As before

private:
    Database() = default;  // Compiler generated default constructor
    // ... As before
};

int main()
{
    Database db;      // Does not compile: Default initialization
    Database db{};    // Works, since value initialization results in aggregate
                      // initialization, because Database is an aggregate type
