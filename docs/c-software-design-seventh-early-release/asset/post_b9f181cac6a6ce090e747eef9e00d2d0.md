
```cpp
#include <set>

class Person
{
public:
    enum StateChange
    {
        forenameChanged,
        surnameChanged,
        addressChanged
    };

    using PersonObserver = Observer<Person,StateChange>; ⑤

    explicit Person( std::string forename, std::string surname )
        : forename_{ std::move(forename) }
        , surname_{ std::move(surname) }
    {}

    bool attach( PersonObserver* observer ); ⑥
    bool detach( PersonObserver* observer ); ⑦

    void notify( StateChange property ); ⑧

    void forename( std::string newForename ); ⑨
    void surname ( std::string newSurname );
    void address ( std::string newAddress );

    std::string const& forename() const { return forename_; }
    std::string const& surname () const { return surname_; }
    std::string const& address () const { return address_; }

private:
    std::string forename_; ③
    std::string surname_;
    std::string address_;

    std::set<PersonObserver*> observers_; ④
};
```

In this example a Person is merely an aggregation of the three data members forename_, surname_, and address_ (③) (I know, this is a rather simple representation of a person). In addition, a person holds a std::set of registered observers (④). Please note that the observers are registered by means of pointers to instances of PersonObserver (⑤).

This is interesting for two reasons: first, this demonstrates the purpose of the templated Observer class: the Person class instantiates its own kind of observer from the class template. And second, pointers prove to be very useful in this context, since the address of an object is unique. Thus it is common to use the address as a unique identifier for an observer.

“Shouldn’t this be a std::unique_ptr or a std::shared_ptr?” you are asking. No, not in this situation. The pointers merely serve as handles to the registered observers, they should not own the observers. Therefore any owning smart pointer would be the wrong tool in this situation. The only reasonable choice would be a std::weak_ptr, which would allow you to check for dangling pointers. However, a std::weak_ptr is not a good candidate for a key for a std::set (not even with a custom comparator). Although there is ways to still use std::weak_ptr, I will stick to raw pointers. But do not worry, this does not mean we are abandoning the benefits of modern C++. No, using a raw pointer is perfectly valid in this situation. This is also expressed in the C++ Core Guideline F.7:

> For the general use, take T* or T& arguments rather than smart pointers.
> 
> —C++ Core Guideline F.7

Whenever you are interested in getting a notification for a state change of a person, you can register an observer via the attach() member function (⑥). And whenever you are no longer interested in getting notifications, you can deregister an observer via the detach() member function (⑦). These two functions are an essential ingredient of the Observer design pattern and therefore a clear indication of the application of the design pattern:

```cpp
bool Person::attach( PersonObserver* observer )
{
    auto [pos,success] = observers_.insert( observer );
    return success;
}

bool Person::detach( PersonObserver* observer )
{
    return ( observers_.erase( observer ) > 0U );
}
```

You have complete freedom to implement the attach() and detach() functions as you see fit. In this example, we only allow an observer to be registered a single time by means of a std::set. In case you try to register an observer a second time, the function returns false. The same thing happens in case you try to deregister an observer that is not registered. Note that the decision to not allow multiple registrations is my choice for this example. In other scenarios it might be desirable or even necessary to accept duplicate registrations. Either way, the behavior and interface of the subject should of course be consistent in all cases.

Another core function of the Observer design pattern is the notify() member function (8). Whenever some state change occurs, this function is called to notify all registered observers about the change:

```cpp
void Person::notify( StateChange property )
{
    for( auto iter=begin(observers_); iter!=end(observers_); )
    {
        auto const pos = iter++;
        (*pos)->update(*this,property);
    }
}
```

> “Why is the implementation of the notify() function so complicated? Wouldn’t a range-based for loop be completely sufficient?” You are correct, I should explain what is happening here. The given formulation makes sure detach() operations can be detected during the iteration. This may for instance happen if an observer decides to detach itself during the call to the update() function. But I do not claim that this formulation is perfect: unfortunately it is not able to cope with attach() operations. And do not even start to ask about concurrency! So this is just one example why the implementation details of observer can be so tricky.

The notify() function is called in all three setter functions (9). Note that in all three functions we always pass a different tag to indicate which property has changed. This tag may be used by classes deriving from the Observer base class to determine the nature of the change:

```cpp
void Person::forename( std::string newForename )
{
    forename_ = std::move(newForename);
    notify( forenameChanged );
}

void Person::surname( std::string newSurname )
{
    surname_ = std::move(newSurname);
    notify( surnameChanged );
}

void Person::address( std::string newAddress )
{
    address_ = std::move(newAddress);
    notify( addressChanged );
}
```

With these mechanics in place, you are now able to write new kinds of fully OCP conforming observers. For instance, you could decide to implement a NameObserver and an AddressObserver:

```
//---- <NameObserver.h> ----------------

#include <Observer.h>
#include <Person.h>

class NameObserver : public Observer<Person,Person::StateChange>
{
public:
    void update( Person const& person, Person::StateChange property ) override;
};

//---- <NameObserver.cpp> ----------------

#include <NameObserver.h>

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

```cpp
#include <AddressObserver.h>
#include <NameObserver.h>
#include <Person.h>
#include <cstdlib>

int main()
{
    NameObserver nameObserver;
    AddressObserver addressObserver;

    Person homer( "Homer" , "Simpson" );
    Person marge( "Marge" , "Simpson" );
    Person monty( "Montgomery", "Burns" );

    // Attaching observers
    homer.attach( &nameObserver );
    marge.attach( &addressObserver );
    monty.attach( &addressObserver );

    // Updating information on Homer Simpson
    homer.forename( "Homer Jay" );  // Adding his middle name

    // Updating information on Marge Simpson
    marge.address( "712 Red Bark Lane, Henderson, Clark County, Nevada 89011" );

    // Updating information on Montgomery Burns
    monty.address( "Springfield Nuclear Power Plant" );

    // Detaching observers
    homer.detach( &nameObserver );

    return EXIT_SUCCESS;
}
```

After these many implementation details, let us take a step back and look at the bigger picture again. Figure 6-3 shows the dependency graph for this Observer example. Due to the decision to implement the Observer class in form of a class template, the Observer class resides on the highest level of our architecture. This enables you to reuse the Observer class for multiple purposes, for instance for the Person class. The Person class declares its own Observer<Person, Person::StateChange> type and by that, injects the code into its own architectural level. Concrete person observers, e.g. NameObserver and AddressObserver, can subsequently build on this declaration.
