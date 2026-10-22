```
ElectricCar::ElectricCar( /*maybe some engine arguments*/ )
    : Car( std::make_unique<ElectricEngine>( /*engine arguments*/
) )
{}

// ...
```

Rather than implementing the *Bridge* itself, the `ElectricCar` class now inherits from the `Car` base class ①. This inheritance relationship introduces the requirement of initializing the `Car` base by specifying an `Engine`. This task is performed in the `ElectricCar` constructor ①.

# The Pimpl Idiom

There is a much simpler form of the *Bridge* design pattern, which has been very commonly and successfully used in both C and C++ for decades. To see an example, let us consider the following `Person` class:

```
class Person
{
public:
    // ...
    int year_of_birth() const;
    // ... Many more access functions

private:
    std::string forename_;
    std::string surname_;
    std::string address_;
    std::string city_;
    std::string country_;
    std::string zip_;
    int year_of_birth_;
    // ... Potentially many more data members
};
```

A person consists of a lot of data members: `forename`, `surname`, the complete postal address, `year_of_birth`, and potentially many more. And there may be the need to add further data members in the future: a mobile phone number, a Twitter account, or the account information for the next best social media fad. In other words, it stands to reason that the