
design pattern is about the polar opposite: it supports you in reducing physical dependencies and helps to decouple, i.e. it keeps two pieces of functionality, that need to work together, but should not know too many details about each other, at arm’s length.

## A Motivating Example

To explain what I have in mind, consider the following ElectricCar class:

```
//---- <ElectricEngine.h> ----------------

class ElectricEngine
{
public:
    void start();
    void stop();

private:
    // ...
};

//---- <ElectricCar.h> ----------------

#include <ElectricEngine.h>
// ...

class ElectricCar
{
public:
    ElectricCar( /*maybe some engine arguments*/ );

    void drive();
    // ...
private:
    ElectricEngine engine_;  ①

    // ... more car-specific data members (wheels, drivetrain, ...)
};
```

```
//---- <ElectricCar.cpp> ----------------

#include <ElectricCar.h>

ElectricCar::ElectricCar( /*maybe some engine arguments*/ )
  : engine_{ /*engine arguments*/ }
  // ... Initialization of the other data members
{}
```

// ...

As the name suggests, the ElectricCar class is equipped with an ElectricEngine (1). However, while in reality such a car may be pretty attractive, the current implementation details are reason to be concerned: because of the engine_ data member, the <ElectricCar.h> header file needs to include the <ElectricEngine.h> header. The compiler needs to see the class definition of ElectricEngine, because otherwise it would not be able to determine the size of a ElectricCar instance. Including the <ElectricEngine.h> header, however, easily results in transitive, physical coupling: every file that includes the <ElectricCar.h> header will physically depend on the <ElectricEngine.h> header. Thus whenever something in the header changes, the ElectricCar class and potentially many more classes are affected. They might have to be recompiled, retested, and, in the worst case, even redeployed... sigh.

On top of that, this design reveals all implementation details to everyone. “What do you mean? Isn’t it the point of the private section of the class to hide and to encapsulate implementation details?” Yes, it may be private, but the private label is merely an access label. It is not a visibility label. Therefore everything in your class definition (and I actually mean everything) is visible to everyone who sees the ElectricCar class definition. This means, that you cannot change the implementation details of this class without anyone noticing. In particular, this may be a problem, if you need to provide ABI stability, i.e. if the in-memory representation of your class must not change.¹

A somewhat better approach would be to only store a pointer to an ElectricEngine (2):²

```
//---- <ElectricCar.h> ----------------

#include <memory>
// ...
struct ElectricEngine;  // Forward declaration

class ElectricCar
{
public:
    ElectricCar( /*maybe some engine arguments*/ );

    void drive();
    // ...
private:
    std::unique_ptr<ElectricEngine> engine_;  2

    // ... more car-specific data members (wheels, drivetrain,
    ...)
};

//---- <ElectricCar.cpp> ----------------

#include <ElectricCar.h>
#include <ElectricEngine.h>  3

ElectricCar::ElectricCar( /*maybe some engine arguments*/ )
    : engine_{ std::make_unique<ElectricEngine>( /*engine
arguments*/ ) }
    // ... Initialization of the other data members
{}

// ... Other 'ElectricCar' member functions, using the pointer to an
//     'ElectricEngine'.

```

In this case, it is sufficient to provide only a forward declaration to the ElectricEngine class, since the compiler does not need to know the class definition to be able to determine the size of a ElectricCar instance. Also, the physical dependency is gone, since the

<ElectricEngine.h> header has been moved into the source file ③. Hence, from a dependency point of view, this solution is much better. What still remains, is the visibility of the implementation details. Everyone is still able to see that the ElectricCar builds on an ElectricEngine and thus everyone is still implicitly depending on these implementation details. Consequently, any change to these details, as for instance an upgrade to the new PowerEngine, would affect any class that works with the <ElectricCar.h> header file. “And that is bad, right?” Indeed it is, because change is to be expected (see “Guideline 2: Design for Change”). In order to get rid of this dependency and to gain the luxury of being able to easily change the implementation details at any time without anyone noticing, we have to introduce an abstraction. The classic form of abstraction is the introduction of an abstract class:

```
//---- <Engine.h> ----------------

class Engine ④
{
public:
    virtual ~Engine() = default;
    virtual void start() = 0;
    virtual void stop() = 0;
    // ... more engine-specific functions

private:
    // ...
};
```

```
//---- <ElectricCar.h> ----------------

#include <Engine.h>
#include <memory>

class ElectricCar
{
public:
    void drive();
    // ...
private:
    std::unique_ptr<Engine> engine_;  ⑤
```

```
// ... more car-specific data members (wheels, drivetrain,
...)
};

//---- <ElectricEngine.h> ----------------

#include <Engine.h>

class ElectricEngine : public Engine
{
public:
    void start() override;
    void stop() override;

private:
    // ...
};

//---- <ElectricCar.cpp> ----------------

#include <ElectricCar.h>
#include <ElectricEngine.h>

ElectricCar::ElectricCar( /*maybe some engine arguments*/ )
    : engine_{ std::make_unique<ElectricEngine>( /*engine arguments*/ ) } ⑤
    // ... Initialization of the other data members
{}

// ... Other 'ElectricCar' member functions, primarily using the
'Engine'
//      abstraction, but potentially also explicitly dealing with
an
//      'ElectricEngine'.
```

With the Engine base class in place ④, we can implement our ElectricCar class using this abstraction ⑤. No one needs to be aware of the actual type of engine that we use. And no one needs to know when we upgrade our engine. With this implementation, we are able to easily change the implementation details at any time by only modifying the source file ⑥. Therefore, with this approach, we have truly minimized

dependencies on the ElectricEngine implementation. We have made the knowledge about this detail our own, secret implementation detail. And by doing that, we have built ourselves a Bridge.

> NOTE
> As stated in the introduction, counter-intuitively, this Bridge is not about bringing the ElectricCar and Engine classes closer together. On the contrary, it is about separating concerns and about loose coupling. Another example that shows that naming is hard.

## The Bridge Design Pattern Explained

The Bridge design pattern is yet another one of the classic GoF design patterns introduced in 1994. The purpose of a Bridge is to minimize physical dependencies by encapsulating some implementation details behind an abstraction. Hence, in C++, it acts as a compilation firewall, which enables easy change:

> THE BRIDGE DESIGN PATTERN
> Intent: “Decouple an abstraction from its implementation so that the two can vary independently.” (GoF)

In this formulation of the intent, the Gang of Four talks about an “abstraction” and an “implementation”. In our example, the ElectricCar class represents the “abstraction”, while the Engine class represents the “implementation” (see Figure 7-1). Both of these should be able to vary independently, i.e. changes to either one should have no effect on the other. The impediment to easy change are the physical dependencies between the ElectricCar class and its engines. Thus, the idea is to extract and isolate these dependencies: by isolating them in form of the Engine abstraction, by separating concerns and fulfilling the Single-Responsibility Principle (SRP), you gain the flexibility to change, tune or

upgrade the engine in any way you want (see “Guideline 2: Design for Change”). The change is no longer visible in the `ElectricCar` class. As a consequence, it is now easily possible to add new kinds of engines, without the “abstraction” noticing. This adheres to the idea of the *Open-Closed Principle (OCP)* (see “Guideline 5: Design for Extension”).
