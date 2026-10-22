In “Guideline 26: Use CRTP to Introduce Static Type Categories” I have introduced you to the Curiously Recurring Template Pattern (CRTP) design pattern. And I may have given you the impression that CRTP is an old hat, made obsolete by the advent of C++20 concepts. Well, interestingly it is not. At least not entirely. That is, because I have not told you the complete story yet. CRTP may still be of value: just not as a design pattern, but as an implementation pattern. So let us take a detour into the realm of implementation patterns and let me explain.

# A Strong Type Motivation

Consider the following StrongType class template, which represents a wrapper around a any other type for the purpose of creating a unique, named type:¹⁸

```
//---- <StrongType.h> ----------------

#include <utility>

template< typename T, typename Tag >
struct StrongType
{
public:
    using value_type = T;

    explicit StrongType( T const& value ) : value_( value ) {}

    T&       get()       { return value_; }
    T const& get() const { return value_; }

private:
    T value_;
};
```

This class can for instance be used to define the types Meter, Kilometer, and Surname:¹⁹

```
//---- <Distances.h> ----------------

#include <StrongType.h>
```