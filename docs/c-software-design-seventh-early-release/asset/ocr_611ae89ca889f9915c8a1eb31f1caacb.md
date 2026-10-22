However, there is a flip side to this direct dependency injection: this approach may quickly become unwieldy in large call stacks: passing a dependency through several levels of your software stack to make them available at the point they are needed is neither convenient nor intuitive. Additionally, especially in the presence of several Singletons, the solution quickly becomes cumbersome: passing for instance a PersistenceInterface, an Allocator, and the system-wide Configuration through many layers of function calls just to be able to use them on the lowest level truly is not the most elegant approach. For that reason, you may want to combine the ideas of providing a global access point (after all, it is global aspects) and a local dependency injection, for instance by introducing a wrapper function:

```
//---- <Widget.h> ----------------

#include <PersistenceInterface.h>

class Widget
{
public:
    void doSomething( /*some arguments*/ )  ⑫
    {
        doSomething( get_persistence_interface(), /*some arguments*/ );
    }

    void doSomething( PersistenceInterface* persistence, /*some arguments*/ )  ⑬
    {
        // ...
        persistence->read( /*some arguments*/ );
        // ...
    }
};
```

While we still provide the previous doSomething() function (⑫), we now additionally provide an overload that accepts a PersistenceInterface as function argument (⑬). The second function does all the work, whereas the first function now merely acts as a