
template< typename T >
using Meter = StrongType<T,struct MeterTag>;

template< typename T >
using Kilometer = StrongType<T,struct KilometerTag>;

// ...

//---- <Person.h> ----------------

#include <StrongType.h>

using Surname = StrongType<std::string,struct SurnameTag>;

// ...

The use of alias templates for Meter and Kilometer enables you to choose for instance long or double to represent a distance. However, although these types are built on fundamental types or standard library types such as std::string as in case of Surname, they represent distinct types (i.e. strong types) with semantic meaning, that cannot be (accidentally) combined in arithmetic operations such as for example addition:

```
//---- <Main.cpp> ----------------

#include <Distances.h>
#include <cstdlib>

int main()
{
    auto const m1 = Meter<long>{ 120L };
    auto const m2 = Meter<long>{  50L };
    auto const km = Kilometer<long>{ 30L };
    auto const surname1 = Surname{ "Stroustrup" };
    auto const surname2 = Surname{ "Iglberger" };
    // ...

    m1 + km;        // Correctly does not compile! ①
    surname1 + surname2;  // Also correctly does not compile! ②
    m1 + m2;        // Inconveniently this does not compile
```

either. ③
    return EXIT_SUCCESS;
}

Although both Meter and Kilometer are represented via long, it is not be possible to directly add Meter and Kilometer together ①. And this is great: it does not leave any opening for accidental bugs to crawl in. It is also not possible to add two Surnames, although a std::string itself provides an addition operator for the purpose of string concatenation ②. But this is also great: the strong type effectively restricts undesired operations of the underlying type. Unfortunately, this “feature” also prevents the addition of two Meter instances ③. This operation would be desirable, though: it is intuitive, natural, and since the result of the operation would again be of type Meter, it would be physically accurate. In order to make this work, we could implement an addition operator for the Meter type. However, obviously, this would not remain the only addition operator. We would also need one for all the other strong types, such as Kilometer, Mile, Foot, etc. Since all of these implementations would look the same, this would be a violation of the Don’t Repeat Yourself (DRY) principle. Therefore it appears to be reasonable to extend the StrongType class template with an addition operator:

```
template< typename T, typename Tag >
StrongType<T,Tag>
operator+( StrongType<T,Tag> const& a, StrongType<T,Tag> const& b )
{
    return StrongType<T,Tag>( a.get() + b.get() );
}
```

Whereas due to the formulation of this addition operator it is not possible to add two different instantiations of StrongType together (e.g. Meter and Kilometer), it would enable the addition of two instances of the same instantiation of StrongType. “Oh, but I see a problem: while it would now be possible to add two Meters or two Kilometers, it would also be possible to add two Surnames. We don’t want that!” You are correct: this

would be undesirable. What we need instead is a deliberate addition of operations to specific instantiations of StrongType. This is where CRTP comes into play.

## Using CRTP As Implementation Pattern

Instead of directly equipping the StrongType class template with operations, we will provide the operations via mixin classes: Base classes that “inject” the desired operations. These mixin classes will be implemented in terms of the CRTP. Consider for instance the Addable class template, which represent the addition operation:

```
//---- <Addable.h> ----------------

template< typename Derived >
struct Addable
{
    friend Derived& operator+=( Derived& lhs, Derived const& rhs )
    {
        ④
        lhs.get() += rhs.get();
        return lhs;
    }

    friend Derived operator+( Derived const& lhs, Derived const& rhs ) {
        ⑤
        return Derived{ lhs.get() + rhs.get() };
    }
};
```

The name of the template parameters gives it away: Addable is a CRTP base class. Addable only provides two functions, implemented as hidden friends: an addition assignment operator ④ and an addition operator ⑤. Both operators are defined for the specified Derived type and will be injected into the surrounding namespace.20 Thus any class deriving from this CRTP base class will “inherit” two free addition operators:

```
//---- <StrongType.h> ----------------

#include <stdlib>
```

```
#include <utility>

template< typename T, typename Tag >
struct StrongType : private Addable< StrongType<T,Tag> >
{ /* ... */ };

//---- <Distances.h> ----------------

#include <StrongType.h>

template< typename T >
using Meter = StrongType<T,struct MeterTag>;

// ...

//---- <Main.cpp> -------------------

#include <Distances.h>
#include <cstdlib>

int main()
{
    auto const m1 = Meter<long>{ 100 };
    auto const m2 = Meter<long>{ 50 };

    auto const m3 = m1 + m2;  // Compiles and results in 150 meters
    // ...

    return EXIT_SUCCESS;
}
```

“I understand the purpose of the mixin class, but in this form all instantiations of StrongType would inherit an addition operator, even the ones where an addition is not required, right?” Yes, indeed. Therefore we are not finished yet. What we want to do is to selectively add the mixin class to those StrongType instantiations, that need the operation. Our solution of choice is to provide the mixins in form of optional template arguments. For that purpose, we extend the StrongType class template by a pack of variadic template template parameters:²¹

```
//---- <StrongType.h> ----------------

#include <utility>

template< typename T, typename Tag, template<typename> class... Skills >
struct StrongType
  : private Skills< StrongType<T,Tag,Skills...> >...
{ /* ... */ };
```

This extension enables us to individually specify, for each single strong type, which skills are desired. Consider for instance the two additional skills Printable and Swappable:

```
//---- <Printable.h> ----------------

template< typename Derived >
struct Printable
{
  friend std::ostream& operator<<( std::ostream& os, const Derived& d )
  {
    os << d.get();
    return os;
  }
};
```

```
//---- <Swappable.h> ----------------

template< typename Derived >
struct Swappable
{
  friend void swap( Derived& lhs, Derived& rhs )
  {
    using std::swap;  // Enable ADL
    swap( lhs.get(), rhs.get() );
  }
};
```

Together with the Addable skill, we can now assemble strong types equipped with the required and desired skills:

```
//---- <Distances.h> ----------------

#include <StrongType.h>

template< typename T >
using Meter =
    StrongType<T,struct MeterTag,Addable,Printable,Swappable>; ⑥

template< typename T >
using Kilometer =
    StrongType<T,struct KilometerTag,Addable,Printable,Swappable>;
⑦

// ...

//---- <Person.h> --------------------

#include <StrongType.h>
#include <string>

using Surname =
    StrongType<std::string,struct SurnameTag,Printable,Swappable>;
⑧

// ...
```

Both Meter and Kilometer can be added, printed and swapped (see ⑥ and ⑦), while Surname is printable and swappable, but not addable (i.e. does not receive the Addable mixin and therefore does not derive from it) (⑧).

“That’s great. I understand the purpose of the CRTP mixin class in this context. But how is this CRTP example different from previous examples?” Very good question. You are right, the implementation details are very similar. But there are a couple of distinctive differences: note that the CRTP base class does not provide a virtual or protected destructor. Hence, in contrast to previous examples, it is not designed as a polymorphic base class. Also note that in this example it is sufficient, and even preferable, to use the CRTP base class as a private base class, not a public one (⑨).

Thus in this context, the *CRTP* base class does not represent an abstraction, but only an implementation detail. Therefore the *CRTP* does not fulfill the properties of a design pattern and it does not act as a design pattern. It is still is a pattern, no question there, but it merely acts as an implementation pattern in this case.

The major difference in the implementation of the *CRTP* examples is the way we use inheritance. For the *CRTP* design pattern, we use inheritance for the purpose of an abstraction according to the *LSP*: the base class represents the requirements and thus the available and expected behavior of the derived class. User code directly accesses the operations via pointers or references to the base class, which in consequence requires us to provide a `virtual` or `protected` destructor. When implemented this way, *CRTP* becomes a true element of software design, a design pattern.

In contrast, for the *CRTP* implementation pattern we use inheritance for the purpose of technical elegance and convenience. The base class becomes an implementation detail and does not have to be known or used by calling code. Therefore it is not in need of a `virtual` or `protected` destructor. When implemented this way, *CRTP* stays on the level of the implementation details and therefore is an implementation pattern. In this form, however, *CRTP* does not compete with C++20 concepts. On the contrary: in this form *CRTP* is unchallenged, as it represents a unique technique to provide static mixin functionality. For that reason, *CRTP* is still of use today and represents a valuable addition to every C++ developers toolbox.

In summary, *CRTP* is not obsolete, but its value has changed. In C++20, *CRTP* is replaced by concepts and therefore is stepping down as a design pattern. However, it continues to be valuable as an implementation pattern for mixin classes.
