```
{
    // ...
    T other = std::move(value);  // Very bad move (literally)!
    // ...
}
```

From this perspective, the set_persistence_interface() function should be used at the very beginning of the program or at the beginning of a single test - not arbitrarily.

“Shouldn’t we make sure that the set_persistence_interface() function can be called only once?”, you ask. We most certainly could do that, but this would artificially limit its use for testing purposes: we would not be able to reset the persistance system at the beginning of every single test.

# Moving Towards Local Dependency Injection

“Ok, I see. One last question: since this solution involves global state that can be changed, wouldn’t it be better to use a more direct and more local dependency injection to the lower-level classes? Consider the following modification of the Widget class, which is given its dependency upon construction”:

```
//---- <Widget.h> ----------------

#include <PersistenceInterface.h>

class Widget
{
public:
    Widget( PersistenceInterface* persistence )  // Dependency injection
        : persistence_(persistence)
    {}

    void doSomething( /*some arguments*/ )
    {
        // ...
        persistence_->read( /*some arguments*/ );
        // ...
    }
```