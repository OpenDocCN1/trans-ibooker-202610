
While most *idioms* fall into the category of *Implementation Details*, there are also *idioms* that fall into the category of *Software Design*. Two examples are the *Non-Virtual Interface Idiom (NVI)* and the *Pimpl Idiom*. These two are based on two classic design patterns, the *Template Method* design pattern and the *Bridge* design pattern, respectively.¹⁵ As such they introduce an abstraction and help to decouple and to design for change and extensions.

## The Focus on Features

If software architecture and software design is of such importance, then why are we in the C++ community focusing so strongly on features? Why do we create the illusion that C++ standards, language mechanics and features are decisive for a project? I think there are three strong reasons for that. First, because there are so many features, with sometimes complex details, we need to spent a lot of time talking about how to use all of them properly. We need to create a common understanding on which use is good and which use is bad. We as a community need to develop a sense of idiomatic C++.

The second reason is that we might put the wrong expectations on features. As an example, let us consider C++20 modules. Without going into details, this feature may indeed be considered as the biggest technical revolution since the very beginning of C++. Modules may at last put the questionable and cumbersome practice of including header files into source files to an end. Due to this potential, the expectations towards that feature are enormous. Some people even expect modules to save their project by fixing their structural issues. Unfortunately, modules will have a hard time to satisfy these expectations: modules do not improve the structure or design of your code, but can merely represent the current structure and design. Modules do not repair your design issues, but they may be able to make the flaws visible. Thus modules simply cannot save your project. So indeed, we may be putting too many or the wrong expectations on features.

And last but not least, the third reason is that despite the huge amount of features and their complexity, in comparison to the complexity of software

design, the complexity of C++ features is small. It is much easier to explain a given set of rules for features, regardless on how many special cases they contain, than explaining the best way to decouple software entities. While there is usually a good answer to all feature-related questions, the common answer in software design is “it depends”. That answer might not even be evidence of inexperience, but of the realization that the best way to make code more maintainable, more changeable, extensible, testable and scalable, heavily depends on many project-specific factors. The decoupling of the complex interplay between many entities may indeed be one of the most challenging endeavors that mankind has ever faced:¹⁶

> *Design and programming are human activities; forget that and all is lost.*
> —Bjarne Stroustrup

To me, it is a combination of these three reasons why we focus on features so much. But please: do not get me wrong. That is not to say that features are not important. On the contrary: feature are important. And yes, it is necessary to talk about features and to learn how to use them correctly, but, once again: they alone do not save your project.

## The Focus on Software Design and Design Principles

While features are important and while it is of course good to talk about them, software design is more important. Software design is essential. I would even argue it is the foundation of the success of our projects. Therefore, in this book I will make the attempt to truly focus on software design and design principles instead of features. Of course I will still show good and up-to-date C++ code, but I will not make the attempt to force the use of the latest and greatest language additions.¹⁷ That means, that I *will* make use of some new features, when it is reasonable and beneficial, as for instance C++20 concepts, but I will *not* pay attention to `noexcept`, or use `constexpr` everywhere.¹⁸ Instead I will try to tackle the difficult aspects of software: I will, for the most part, focus on software design, the rationale behind design decisions, design principles, on managing dependencies and on dealing with abstractions.

In summary, software design is the critical part of writing software.
Software developers are well advised to have a good understanding of software design in order to be able to write good, maintainable software.
Because after all, good software is low-cost, bad software is expensive.

## GUIDELINE 1: UNDERSTAND THE IMPORTANCE OF SOFTWARE DESIGN

- Treat software design as an essential part of writing software;
- Focus less on C++ language details and more on software design;
- Avoid unnecessary coupling and dependencies to make software more adaptable to frequent changes;
- Understand software design as the art of managing dependencies and abstractions;
- Consider the boundary between software design and software architecture as fluent.

## Guideline 2: Design for Change

One of the essential expectations for good software is its ability to change easily. This expectation is even part of the word *software*. *Software*, in contrast to *hardware*, is expected to be able to adapt easily to changing requirements (see also “Guideline 1: Understand the Importance of Software Design”). However, from your own experience you may be able to tell that often it is not easy to change code. On the contrary, sometimes a seemingly simple change turns out to be a week-long endeavor.

## Separation of Concerns

One of the best and proven solutions to reduce artificial dependencies and to simplify change is to separate concerns. The core of the idea is to split,

segregate, or extract pieces of functionality:¹⁹

> *Systems that are broken up into small, well-named, understandable pieces enable faster work.*
> 
> —Michael Feathers

The intent behind separation of concerns is to better understand and manage complexity and thus design more modular software. This idea is probably as old as software itself and hence has been given many different names. For instance, the same idea is called *Orthogonality* by the Pragmatic Programmers.²⁰ They advise to separate orthogonal aspects of software. Tom DeMarco called it *Cohesion*:²¹

> *Cohesion is a measure of the strength of association of the elements inside a module. A highly cohesive module is a collection of statements and data items that should be treated as a whole because they are so closely related. Any attempt to divide them up would only result in increased coupling and decreased readability.*
> 
> —Tom DeMarco

In the *SOLID* principles, one of the most established sets of design principles, the idea is known as the *Single-Responsibility Principle (SRP)*.²²

> *A class should have only one reason to change.*
> 
> —Robert C. Martin

Although the concept is old and although it is commonly known under many names, many attempts to explain separation of concerns raise more questions than they give answers. This is particularly true for the *SRP*. The name of this design principle alone raises questions: what is a responsibility? And what is a *single* responsibility? A common attempt to clarify the vagueness about *SRP* is the following:

> *Everything should do just one thing.*
> 
> —Common Wisdom?

Unfortunately this explanation is hard to outdo in terms of vagueness. Just as the word *responsibility* does not carry a lot of meaning, *just one thing*

does not help to shed any more light on it.

Irrespective of the name, the idea is always the same: group only those things that truly belong together, and separate everything that does not strictly belong. Or in other words: separate those things that change for different reasons. By doing this, you reduce artificial coupling between different aspects of your code and it helps you to make your software more adaptable to change. In the best case, you can change a particular aspect of your software in exactly one place.

## An Example for Artificial Coupling

Let us shed some light on separation of concerns by means of a code example. And I do have a great example indeed: I present to you the abstract Document class:

```
//#include <some_json_library.h> // Potential physical dependency

class Document
{
public:
    // ...
    virtual ~Document() = default;

    virtual void exportToJSON( /*...*/ ) const = 0; ①
    virtual void serialize( ByteStream&, /*...*/ ) const = 0; ②
    // ...
};
```

This sounds like a very useful base class for all kinds of documents, doesn’t it? First, there is the exportToJSON() function (①). All deriving classes will have to implement the exportToJSON() function in order to produce a JSON file from the document. That will prove to be pretty useful: without having to know about a particular kind of document (and we can imagine that we will eventually have PDF documents, Word documents, and many more) we can always export in JSON format. Nice! Second, there is a serialize() function (②). This function lets you transform a
