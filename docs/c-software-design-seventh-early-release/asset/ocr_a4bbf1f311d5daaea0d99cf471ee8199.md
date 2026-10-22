```
onUpdate_( subject, property );
}

private:
    OnUpdate onUpdate_;
```

Instead of implementing the Observer class as a base class, and thus requiring deriving classes to inherit and to implement the update() function in a very specific way, we separate concerns and rather build on composition (see “Guideline 20: Favor Composition Over Inheritance”). The Observer class first provides a type alias called OnUpdate for a std::function type for the expected signature of our update() function ⑪. Via the constructor you are passed an instance of std::function ⑩ and you move it into your data member onUpdate_ ⑧. The job of the update() function is now to forward the call, including the arguments, to onUpdate_ ⑧.

The flexibility gained by means of std::function is easily demonstrated by means of an updated main() function:

```
#include <Observer.h>
#include <Person.h>
#include <cstdlib>

void propertyChanged( Person const& person, Person::StateChange property )
{
    if( property == Person::forenameChanged ||
        property == Person::surnameChanged )
    {
        // ... Respond to changed name
    }
}

int main()
{
    using PersonObserver = Observer<Person,Person::StateChange>;

    PersonObserver nameObserver( propertyChanged );

    PersonObserver addressObserver(
```