
+   - Function parameters in a subtype must be contravariant: in a member function, the subtype can accept a super type of the function parameter in the according member function of the super type. This property does not have direct language support in the C++ language.

```
struct Base { /*...some virtual functions, including destructor...*/ };
struct Derived : public Base { /*...*/ };
```

```
struct X
{
    virtual ~X() = default;
    virtual void f( Derived* );
};
```

```
struct Y : public X
{
    void f( Base* ) override;  // Contravariant function parameter; Not
                        // supported in C++. Therefore the
                        // function
                        // does not override, but fails to
                        // compile.
};
```

+   - Invariants of the super type must be preserved in a subtype: any expectation about the state of a super type must always be valid before and after all calls to any member function, including the member functions of the subtype.

```
struct X
{
    explicit X( int v = 1 )
        : value_(v)
    {
        if( v < 1 || v > 10 ) throw std::invalid_argument( /*...*/ );
    }

    virtual ~X() = default;

    int get() const { return value_; }
```

```
protected:
    int value_;  // Invariant: must be within the range [1..10]
};

struct Y : public X
{
public:
    Y()
        : X()
    {
        value_ = 11;  // Broken invariant: After the constructor,
'value_'               // is out of expected range. One good reason
to                     // properly encapsulate invariants and to
follow                 // Core Guideline C.133: Avoid protected
data.                  
    }
};
```

In our example, the expectation in a Rectangle is that we can change the two side lengths independently, or, more formally, that the result of getWidth() does not change after setHeight() is called. This expectation is intuitive for any kind of rectangle. However, the Square class itself introduces the invariant that all sides must always be equal, or else the Square would not properly express our idea of a square. But by protecting its own invariants, the Square unfortunately violates the expectations in the base class. Thus, the Square class does not fulfill the expectations in the Rectangle class and the hierarchy in this example does not express an IS-A relationship. A Square can therefore not be used in all the places a Rectangle is expected.

“But isn’t a square a rectangle?”, you are asking. “Isn’t that properly expressing the geometrical relation?”⁴ Yes, there may be a geometrical relation between squares and rectangles, but in this example the inheritance relationship is broken. This example demonstrates that the mathematical IS-A relation is indeed different from the LSP IS-A relation. While in geometry a square is always a rectangle, in computer science it really depends on the actual interface and thus the expectations. As long as there are the two

independent `setWidth()` and `setHeight()` functions, a `Square` will always violate the expectations. “I understand”, you say. “Nobody would claim that, geometrically, a square is still a square after changing its width, right?” Exactly.

The example also demonstrates that inheritance is not a natural or intuitive feature, but a hard feature. As stated in the beginning, building abstractions is hard. Whenever you use inheritance, you *must* make sure that all expectations in the base class are fulfilled and that the derived type behaves as expected.

## Criticism about the Liskov Substitution Principle

Some people argue that the *LSP*, as explained above, is in fact not what is described in the 1987 OOPSLA conference paper “Data Abstraction and Hierarchy” by Barbara Liskov and that the notion of subtyping is flawed. And that is correct: we usually do not substitute derived objects for base objects, but we use a derived object as a base object. However, this literal and strict interpretation of Liskov’s statements does not play any role for the kinds of abstractions that we build on a daily basis. In their 1994 paper “A behavioral notion of subtyping”, Barbara Liskov and Jeannette Wing proposed the term *behavioral subtyping*, which is the common understanding of the *LSP* today.⁵

Some other people argue, that because of potential violations of *LSP*, a base class does not serve the purpose of an abstraction. The rationale is, that using code would also depend on the (mis-)behavior of derived types. This argument unfortunately turns the world upside down. A base class *does* represent an abstraction because calling code can and should only and exclusively depend on the *expected* behavior of this abstraction. It is that dependency, that makes *LSP* violations programming errors. Unfortunately, sometimes people try to fix *LSP* violations by introducing special workarounds:

```
class Base { /*...*/ };
class Derived : public Base { /*...*/ };
```

```
class Special : public Base { /*...*/ };
// ... Potentially more derived classes

void f( Base const& b )
{
    if( dynamic_cast<Special const*>( &b ) )
    {
        // ... do something "special", knowing that 'Special'
        behaves differently
    }
    else
    {
        // ... do the expected thing
    }
}
```

This kind of workaround will indeed introduce a dependency to the behavior of the derived types. And a very unfortunate dependency, indeed! This should always be considered an LSP violation and as very bad practice.⁶ It does not serve as a general argument against the abstracting properties of a base class.

## The Need for Good and Meaningful Abstractions

In order to properly decouple software entities, it is fundamentally important that we can count on our abstractions. Without meaningful abstractions, abstractions that we, the human readers of code, fully understand, we cannot write robust and reliable software. Therefore adherence to the LSP is essential for the purpose of software design. However, a vital part of that is also the clear and unambiguous communication of the expectations of an abstraction. In the best case this happens by means of software itself (self-documenting code), but this also entails a proper documentation of abstractions. As a good example, I recommend the documentation of the iterator concepts in the C++ standard, which clearly lists the expected behavior, including pre- and postconditions.

## GUIDELINE 6: ADHERE TO THE EXPECTED BEHAVIOR OF ABSTRACTIONS

+   - Understand that an abstraction represents a set of requirements and expectations;
+   - Follow the *Liskov Substitution Principle (LSP)* to adhere to the expected behavior of abstractions;
+   - Make sure that derived classes adhere to the expected behavior of their base classes;
+   - Communicate the expectations of an abstraction.

## Guideline 7: Understand the Similarities between Base Classes and Concepts

In “Guideline 6: Adhere to the Expected Behavior of Abstractions” I may have created the impression that the *Liskov Substitution Principle (LSP)* is concerned only with inheritance hierarchies and base classes. In order to make sure that this impression does not stick, allow me to explicitly state that the *LSP* is *not* limited to dynamic (runtime) polymorphism and inheritance hierarchies. On the contrary, we can apply the *LSP* just as well to static (compile time) polymorphism and templated code.

To make the point, let me ask you a question: what is the difference between the following two code snippets?

```
//==== Code Snippet 1 ====

class Document
{
public:
    // ...
    virtual ~Document() = default;

virtual void exportToJSON( /*...*/ ) const = 0;
virtual void serialize( ByteStream&, /*...*/ ) const = 0;
// ...
};

void useDocument( Document const& doc )
{
    // ...
    doc.exportToJSON( /*...*/ );
    // ...
}
```

//==== Code Snippet 2 ====

```
template< typename T >
concept Document =
    requires( T t, ByteStream b ) {
        t.exportToJSON( /*...*/ );
        t.serialize( b, /*...*/ );
    };

template< Document T >
void useDocument( T const& doc )
{
    // ...
    doc.exportToJSON( /*...*/ );
    // ...
}
```

I am pretty sure your first answer is that the first code snippet shows a solution using dynamic polymorphism, the second one shows static polymorphism. Yes, great! What else? Ok, yes, of course, the syntax is different, too. Ok, I see, I should ask my question a little more precisely: in which way do these two solutions differ semantically?

Well, if you think about it, then you might find that from a semantical point of view the two solutions are very similar indeed. In the first code snippet, the useDocument() function only works with classes derived from the Document base class. Thus we can say that the the function only works with classes adhering to the expectations of the Document abstraction. In the second code snippet, the useDocument() function only works with classes that implement the Document concept. In other words, the
