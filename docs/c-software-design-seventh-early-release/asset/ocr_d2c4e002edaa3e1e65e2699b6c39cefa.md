```
// ... Potentially many more data members
};
```

The sole task of the nested Impl class is to encapsulate the implementation details of Person. Thus, the only data member remaining in the Person class is a std::unique_ptr to an Impl instance (⑧). All other data members, and potentially some non-virtual helper functions, are moved from the Person class into the Impl class. Note that the Impl class is only declared in the Person class, but not defined. Instead, it is defined in the corresponding source file (⑨). Only due to this, all details and all changes that you apply to the details, i.e. adding or removing data members, changing the type of data members, etc., are hidden from the users of Person.

This implementation of Person uses the Bridge design pattern in its simplest form: this local, non-polymorphic form of Bridge is called the Pimpl Idiom. It comes with all the decoupling advantages of the Bridge pattern, but despite its simplicity, still results in a bit more complex implementation of the Person class:

```
//---- <Person.h> --------------------

// #include <memory>

class Person
{
public:
    // ...
    Person();      ⑮
    ~Person();      ⑯

    Person( Person const& other );      ⑰
    Person& operator=( Person const& other );      ⑱

    Person( Person&& other );      ⑲
    Person& operator=( Person&& other );      ⑳

    int year_of_birth() const;      ㉑
    // ... Many more access functions
```