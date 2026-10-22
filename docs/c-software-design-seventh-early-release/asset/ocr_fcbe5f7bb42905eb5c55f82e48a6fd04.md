```
void NameObserver::update( Person const& person,
Person::StateChange property )
{
    if( property == Person::forenameChanged ||
        property == Person::surnameChanged )
    {
        // ... Respond to changed name
    }
}
```

```
//---- <AddressObserver.h> ----------------

#include <Observer.h>
#include <Person.h>

class AddressObserver : public
Observer<Person,Person::StateChange>
{
public:
    void update( Person const& person, Person::StateChange
property ) override;
};
```

```
//---- <AddressObserver.cpp> ----------------

#include <AddressObserver.h>

void AddressObserver::update( Person const& person,
Person::StateChange property )
{
    if( property == Person::addressChanged ) {
        // ... Respond to changed address
    }
}
```

Equipped with these two observers, you are now able to get notified whenever either the name or address of a person changes:

```
#include <AddressObserver.h>
#include <NameObserver.h>
#include <Person.h>
#include <cstdlib>

int main()
```