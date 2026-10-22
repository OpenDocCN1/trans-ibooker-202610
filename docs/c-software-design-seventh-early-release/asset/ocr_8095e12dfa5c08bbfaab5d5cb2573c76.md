Figure 6-3. Dependency graph for the Observer design pattern.

# An Observer Implementation Based on Value Semantics

“I understand why you have started with a classic implementation, but since you have made the point about favoring value semantics, how would the observer look like in a value semantics world?” That is an excellent question, since this a very reasonable next step. As explained in “Guideline 22: Prefer Value Semantics to Reference Semantics”, there are a lot of good reasons to avoid the realm of reference semantics. However, we will not entirely stray from the classic implementation: in order to register and deregister observers we will always be in need of some unique identifier for observers and the unique address of an observer is just the easiest and most convenient way to tackle that problem. Therefore we will stick to using a pointer to refer to a registered observer. However, there is an elegant way to avoid the inheritance hierarchy: std::function:

```
//---- <Observer.h> ----------------

#include <functional>

template< typename Subject, typename StateTag >
class Observer
{
public:
    using OnUpdate = std::function<void(Subject const&, StateTag)>;

    // No virtual destructor necessary

    explicit Observer( OnUpdate onUpdate )
        : onUpdate_{ std::move(onUpdate) }
    {
        // Possibly respond on an invalid/empty std::function instance
    }

    // Non-virtual update function
    void update( Subject const& subject, StateTag property )
    {
```