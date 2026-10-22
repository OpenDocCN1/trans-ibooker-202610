Unfortunately, a particular subject is not likely to distinguish between different kinds of observers. First, because this would require it to store different kinds of pointers (which is inconvenient to handle for the subject) and second, because it is possible that different state changes are linked in a certain way. In that case, the subject will expected that observers are interested in *all* possible state changes. From that perspective it can be reasonable to combine several `update()` functions into one base class. Either way, it is very likely that a concrete observer will have to deal with all kinds of state change. I know, it can be a nuisance to have to deal with several `update()` functions, even if only a small fraction of them are interesting. But still, make sure that you are not accidentally violating the *Liskov Substitution Principle* by not adhering to some expected behavior (if there is any).

There are several more potential downsides of a *push observer*. First, the observers are *always* given *all* the information, whether they need it or not. Thus this push style works well only if the observers need the information most of the time. Otherwise a lot of effort is lost on unnecessary notifications. Second, pushing creates a dependency on the number and kind of arguments that are passed to the observer. Any change to these arguments requires a lot of subsequent changes in the deriving observer classes.

Some of these downsides are resolved by the second `Observer` alternative. It is possible to only pass a reference to the subject to the observer:¹⁰

```
class Observer
{
public:
    // ...
    virtual void update( Subject const& subject ) = 0;
    // ...
};
```

Due to the lack of specific information passed to the observer, the classes deriving from the `Observer` base class are required to pull the new