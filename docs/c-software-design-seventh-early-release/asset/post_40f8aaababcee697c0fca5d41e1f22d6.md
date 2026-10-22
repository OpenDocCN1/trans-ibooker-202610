
## Table 4-1. Strengths and weaknesses of different programming paradigms

| Programming Paradigm | Strength | Weakness |
|---|---|---|
| Procedural Programming | Addition of operations | Addition of (polymorphic) types |
| Object-Oriented Programming | Addition of (polymorphic) types | Addition of operations |

Be aware of these strengths: based on your expectation on how a code base will evolve, choose the right approach to design for extensions. Do not ignore the weaknesses and do not put yourself in an unfortunate maintenance hell.

I assume that at this point, you are wondering if it is possible to have two *open sets*. Well, to the best of my knowledge this is not impossible, but usually impractical. As an example, in “Guideline 18: Beware the Performance of Acyclic Visitors” I will show you that performance might take a significant hit.

Since you might be a fan of template-based programming and similar compile time endeavors, I should also make the explicit note that static polymorphism does not have the same limitations. While in dynamic polymorphism one of the design axes (types and operations) needs to be fixed, in static polymorphism both pieces of information are available at compile time. Therefore both aspects can be extended easily (if you do it properly).⁵

## GUIDELINE 15: DESIGN FOR THE ADDITION OF TYPES OR OPERATIONS

- Be aware of the strengths and weaknesses of different programming paradigms;
- Exploit the strengths of a paradigm, but avoid the weaknesses;
- Understand the choice between the addition of types or operations in dynamic polymorphism;
- Prefer an object-oriented solution when you primarily want to add types;
- Prefer a procedural/functional solution when you primarily want to add operations.

## Guideline 16: Use Visitors to Extend Operations

In the last section you saw that the strength of object-oriented programming (OOP) is the addition of types and that its weakness is the addition of operations. But of course OOP has an answer to that weakness: the *Visitor* design pattern.

The *Visitor* design pattern is one of the classic design patterns described by the Gang of Four (GoF). Its focus is on allowing you to frequently add operations instead of types. Allow me to explain the *Visitor* design pattern by means of the previous toy example: the drawing of shapes.

In Figure 4-1 you see the `Shape` hierarchy. The `Shape` class is again the base class for a certain number of concrete shapes. In this example, there are only the two classes `Circle` and `Square`, but of course it is possible to have more shapes. In addition, you might imagine `Triangle`, `Rectangle` or `Ellipse` classes.

![](img/7b1b8ee332752e5702cc436a0cba7c1c_180_0.png)

Figure 4-1. The UML representation of a shape hierarchy with two derived classes (Circle and Square).

### Analyzing the Design Issues

Let us assume you are certain that you already have all the shapes that you will ever need. That is, you consider the set of shapes a closed set. What you are missing, though, are additional operations. For instance, you are

missing an operation to rotate the shapes. Also, you would like to serialize shapes, i.e. you would like to convert the instance of a shape into bytes. And of course you want to draw shapes. In addition you want to enable anybody to add new operations. Therefore you expect an open set of operations.⁶

Every new operation now requires you to insert a new virtual function into the base class. Unfortunately that can be troublesome in different ways. Most obviously, not everyone is able to add a virtual function to the Shape base class. I, for instance, can not simply go ahead and change your code. Therefore this approach would not meet the expectation that everyone can add operations. While you could already see this as a final negative verdict, let us still analyse the problem of virtual functions in more detail.

If you decide to use a pure virtual function, you would have to implement the function in every derived class. For your own derived types you could shrug this off as just a little bit of extra effort. But you might also cause extra work for other people who have created a shape by inheriting from the Shape base class.⁷ And that is very much expected, since this is the strength of OOP: anyone can add new types easily. Since this is to be expected, that may be a reason to not to use a pure virtual function.

As an alternative, you could introduce a regular virtual function, i.e. a virtual function with a default implementation. While a default behavior for a rotate() function sounds like a very reasonable idea, a default implementation for a serialize() function does not sound easy at all. I admit that I myself would have to think hard about how to implement such a function. You might now suggest to just throw an exception as default. However, this essentially means that derived classes must again implement the missing behavior, it would only be a pure virtual function in disguise. Or a clear violation of the Liskov Substitution Principle (LSP) (see “Guideline 6: Adhere to the Expected Behavior of Abstractions”).

Either way, adding a new operation into the Shape base class is difficult or not even possible at all. The underlying reason is that adding virtual functions violates the Open-Closed Principle (OCP). If you really need to

add new operations frequently, then you should design such that the extension of operations is easy. And that is what the *Visitor* design pattern tries to achieve.

### The Visitor Design Pattern Explained

> **THE *VISITOR* DESIGN PATTERN**
> 
> Intent: “Represent an operation to be performed on the elements of an object structure. *Visitor* lets you define a new operation without changing the classes of the elements on which it operates.” (GoF)

In addition to the `Shape` hierarchy, we now introduce the `ShapeVisitor` hierarchy on the left-hand side of Figure 4-2. The `ShapeVisitor` base class represents an abstraction of shape operations. For that reason, you could argue that `ShapeOperation` might be a better name for that class. It is beneficial, however, to apply “Guideline 14: Use a Design Pattern’s Name to Communicate Intent”. The name *Visitor* will help others to understand the design.

![](img/7b1b8ee332752e5702cc436a0cba7c1c_183_0.png)

Figure 4-2. The UML representation of the Visitor design pattern.

The ShapeVisitor base class comes with one pure virtual visit() function for every concrete shape in the Shape hierarchy:

```
class ShapeVisitor
{
public:
virtual ~ShapeVisitor() = default;

virtual void visit( Circle const&, /*...*/ ) const = 0; ①
virtual void visit( Square const&, /*...*/ ) const = 0; ②
// Possibly more visit() functions, one for each concrete shape
};
```

In this example, there is one visit() function for Circle (①) and one for Square (②). There could of course be more visit() functions, for instance one for Triangle, one for Rectangle, and one for Ellipse, given that these are also classes derived from the Shape base class.

With the ShapeVisitor base class in place, you are now able to add new operations easily. All you have to do to add an operation, is to add a new derived class. For instance, in order to enable rotating shapes, you can introduce the Rotate class and implement all visit() functions. In order to enable drawing shapes, all you have to do is to introduce a Draw class:

```
class Draw : public ShapeVisitor
{
public:
void visit( Circle const& c, /*...*/ ) const override;
void visit( Square const& s, /*...*/ ) const override;
// Possibly more visit() functions, one for each concrete shape
};
```

And you can think about introducing multiple Draw classes, one for each graphics library you need to support. You can do that easily, because you do

not have to modify any *existing code*. It is only necessary to extend the `ShapeVisitor` hierarchy by adding *new code*. Therefore this design fulfills the *OCP* with respect to adding operations.

In order to completely understand the software design characteristics of *Visitor*, it is important to understand why the *Visitor* design pattern is able to fulfill the *OCP*. The initial problem was, that every new operation would require a change to the `Shape` base class. *Visitor* identifies the addition of operations as a *variation point*. By extracting this *variation point*, i.e. by making this a separate class, you follow the *Single-Responsibility Principle* (*SRP*): `Shape` does not have to change for every new operation. This avoids frequent modifications of the `Shape` hierarchy and enables the easy addition of new operations. *SRP* therefore acts as an enabler for the *OCP*.

In order to be able to use visitors (i.e. classes derived from the `ShapeVisitor` base class) on shapes, you now have to add one last function to the `Shape` hierarchy: the `accept()` function (③):⁸

```
class Shape
{
  public:
    virtual ~Shape() = default;
    virtual void accept( ShapeVisitor const& v ) = 0;  ③
    // ...
};
```

The `accept()` function is introduced as a pure virtual function in the base class and therefore has to be implemented in every derived class (④ and ⑤):

```
class Circle : public Shape
{
  public:
    explicit Circle( double radius )
      : radius_( radius )
    {
      /* Checking that the given radius is valid */
    }
```
