to the *Observer* design pattern, one of the most famous and most commonly used design patterns. We will talk about the classic, GoF-style *Observer*, but also about how to implement *Observers* in modern C++.

In “Guideline 26: Use CRTP to Introduce Static Type Categories”, we will turn our attention to the *CRTP*. I will show you how to use *CRTP* to define a compile time relationship between a family of related types and how to properly implement a *CRTP* base class.

In “Guideline 27: Use CRTP for Static Mixin Classes”, I will continue the *CRTP* story by showing you how *CRTP* can be used to create compile time mixin classes. We will also see the difference between semantical inheritance, where it is used for the purpose of creating an abstraction, and technical inheritance, where it is used as an implementation detail for only technical elegance and convenience.

# Guideline 24: Use Adapters to Standardize Interfaces

Let us assume that you have implemented the `Document` example from “Guideline 3: Separate Interfaces to Avoid Artificial Coupling”, and that, because you properly adhere to the *ISP*, you are reasonably happy with the way it works:

```
class JSONExportable
{
public:
    // ...
    virtual ~JSONExportable() = default;

    virtual void exportToJSON( /*...*/ ) const = 0;
    // ...
};

class Serializable
{
public:
    // ...
    virtual ~Serializable() = default;
```