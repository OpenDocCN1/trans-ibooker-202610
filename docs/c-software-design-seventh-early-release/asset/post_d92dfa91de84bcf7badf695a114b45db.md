
//---- <PersistenceInterface.cpp> ----------------

#include <Database.h>

// Definition of the one 'instance' variable
PersistenceInterface* instance = nullptr;

PersistenceInterface* get_persistence_interface()
{
    // Local object, initialized by an 'Immediately Invoked Lambda Expression (IILE)'
    static bool init = [](){
        if (!instance) {
            static Database db;
            instance = &db;
        }
        return true; // or false, as the actual value does not matter.
    }(); // Note the '()' after the lambda expression. This invokes the lambda.

    return instance;
}

void set_persistence_interface( PersistenceInterface* persistence )
{
    instance = persistence;
}
```

The first time the execution flow enters the get_persistence_interface() function, the init static local variable is initialized. If, at this point in time, the instance is already set, no Database is created. However, if it is not, the Database instance is created as another static local variable inside the lambda and bound to the instance variable.

```
#include <PersistenceInterface.h>
#include <cstdlib>

int main()
{
    // First access, database object is created
    PersistenceInterface* persistence =
get_persistence_interface();

// ...

return EXIT_SUCCESS;
}
```

This implementation achieves the desired effect: Database becomes an implementation detail, which no other code depends on and which can be replaced at any time by means of a custom database implementation (see Figure 10-5). Thus, despite the Singletonness of Database, it does not introduce dependencies, it can be easily changed and easily replaced for testing purposes.

## High-level

```
class PersistenceInterface
{
public:
    virtual ~PersistenceInterface() = default;
    // ...
private:
    virtual bool do_read( /* some arguments */ ) const;
    virtual bool do_write( /* some arguments */ );
};
```

```
class Database : public PersistenceInterface
{
public:
    // ...
private:
    bool do_read( /*...*/ ) const override;
    bool do_write( /*...*/ ) override;
    // ...
};
```

Architectural Boundary

```
class Widget
{
    // ...
};
```

Architectural Boundary

```
class Gadget
{
    // ...
};
```

```
class CustomPersistence
: public PersistenceInterface
{
public:
    // ...
private:
    bool do_read( /*...*/ ) const override;
    bool do_write( /*...*/ ) override;
    // ...
};
```

Low-level

No dependencies on the Database class!

Figure 10-5. The dependency graph for the refactored, non-Singleton Database.

“Wow, this is a great solution. I bet I can use that in a few places in my own code base!” you state, with an impressed and appreciative look on your face. “But I see a potential problem: since I have to inherit from an interface class, this is an intrusive solution. What should I do in case I cannot change a given Singleton class?” Well, in that case you have two non-intrusive design patterns to choose from: either you already have an inheritance hierarchy in place, in which case you can introduce an Adapter to wrap the given Singleton (see “Guideline 24: Use Adapters to Standardize Interfaces”). Or you do not have an inheritance hierarchy in place yet, in which case you can put the External Polymorphism design pattern to good use (see “Guideline 31: Use External Polymorphism for Non-Intrusive Runtime Polymorphism”).

“Ok, but I see another, more serious problem: is this code truly thread-safe?” Honestly: no, it is not. To give one example for a possible problem: it could happen that during the first call to get_persistence_interface(), which may take some time due to the setup of the Database instance, the set_persistence_interface() is called. In that case, either the Database is created in vain or the call to set_persistence_interface() is lost. However, perhaps surprisingly, this is not something that we need to address. Here is why: remember that the instance represents global state. If we assume that set_persistence_interface() can be called from anywhere in the code at any time, in general we cannot expect that after calling set_persistence_interface() a call to get_persistence_interface() would return the set value. Hence calling the set_persistence_interface() function from anywhere in the code is like pulling the rug from under somebody’s feet. This is comparable to calling std::move() on any lvalue:

```
template< typename T >
void f( T& value )
{
    // ...
    T other = std::move(value);  // Very bad move (literally)!
    // ...
}
```

From this perspective, the set_persistence_interface() function should be used at the very beginning of the program or at the beginning of a single test - not arbitrarily.

“Shouldn’t we make sure that the set_persistence_interface() function can be called only once?”, you ask. We most certainly could do that, but this would artificially limit its use for testing purposes: we would not be able to reset the persistence system at the beginning of every single test.

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

}
private:
    PersistenceInterface* persistence_{};
};
```

I completely agree with you. This may be the next step to address the problem of global state. However, before we analyze this approach, you should realize that this idea is only an option since we have already inverted the dependencies: thanks to introducing an abstraction in the high-level of our architecture we suddenly have choices and can talk about alternative solutions. Hence the first and most important step is to properly manage the dependencies. But back to your suggestion: I really like the approach. The interface of the Widget class becomes more “honest” and clearly displays all of its dependencies. And since the dependency is passed via constructor argument the dependency injection becomes more intuitive and more natural.

Alternatively you could pass the dependency to the Widget::doSomething() function directly:

```
//--- <Widget.h> -------------

#include <PersistenceInterface.h>

class Widget
{
public:
    void doSomething( PersistenceInterface* persistence, /*some arguments*/ )
    {
        // ...
        persistence->read( /*some arguments*/ );
        // ...
    }
};
```

While this approach may not be the best for a member function, this may be your only option for free functions. And again, the function becomes a little more “honest” by explicitly stating its dependencies.

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
