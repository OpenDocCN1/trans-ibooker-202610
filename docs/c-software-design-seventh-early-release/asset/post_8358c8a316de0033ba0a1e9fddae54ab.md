
```
[index]; }
    ??? begin()      { return derived().begin(); }
    ??? begin() const { return derived().begin(); }
    ??? end()        { return derived().end(); }
    ??? end()    const { return derived().end(); }
    // ...
};
```

However, these functions are not as straightforward as the size() member function. In particular, the return types prove to be a little harder to specify, as these types depend on the implementation of the Derived class. “Well, that shouldn’t be too hard”, you state, “this is the reason why the derived types provide a couple of nested types, such as value_type, iterator, and const_iterator, right?” Indeed, it appears to be intuitive to just ask nicely:

```
template< typename Derived >
struct DenseVector
{
    // ...
    using value_type     = typename Derived::value_type;
    using iterator       = typename Derived::iterator;
    using const_iterator = typename Derived::const_iterator;
    value_type&     operator[]( size_t index )      { return derived()[index]; }
    value_type const& operator[]( size_t index ) const { return derived()[index]; }
    iterator        begin()      { return derived().begin(); }
    const_iterator  begin() const { return derived().begin(); }
    iterator        end()        { return derived().end(); }
    const_iterator  end()    const { return derived().end(); }
    // ...
};
```

We query for the value_type, iterator, and const_iterator types in the derived class (do not forget the typename keyword) and use

these to specify our return types (15). Easy, right? Oh well ... you can almost bet that it is not that easy. If you try this, the Clang compiler will complain with a seriously weird and baffling error message:

```
CRTP.cpp:29:41: error: no type named 'value_type' in
'DynamicVector<int>'
using value_type = typename Derived::value_type;
~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~
```

“No value_type in DynamicVector<int>... strange.” The first idea that crosses your mind is that you messed up. It must be a typo. Of course! So you go back to your code and check the spelling. However, it turns out that everything seems to be OK. There is no typo. You check the DynamicVector class again... there it is, the nested value_type member. And everything is public, too... The error message just does not make any sense... You reexamine everything, and again, and half an hour later you conclude: “The compiler has a bug!”

No, it is not a bug in the compiler. Not in Clang, nor any other compiler. GCC provides a different, still slightly puzzling, but a perhaps little more illuminating error message:16

```
CRTP.cpp:29:10: error: invalid use of incomplete type 'class DynamicVector<int>'
 29 |     using value_type = typename Derived::value_type;
     |         ^~~~~~~~~~~
```

The Clang compiler is correct: there is no value_type in the DynamicVector class. Not yet! At the point in time you query for the nested types, the definition of the DynamicVector class has not been seen and DynamicVector is still an incomplete type. That is because the compiler will instantiate the DenseVector base class before the definition of the DynamicVector class. After all, syntactically the base class is specified before the body of the class:

```
template< typename T >
class DynamicVector : public DenseVector<DynamicVector<T>>
```

// ...

In consequence, there is no way that you can use the nested types of the derived class for the return types of the CRTP class. In fact, you cannot use anything as long as the derived class is an incomplete type. “But why can I call the member functions of the derived class? Shouldn’t this result in the same problem?” Luckily, this works (otherwise the CRTP pattern would not work at all). But it only works because of a special property of class templates: member functions are only instantiated on demand, meaning when they are actually called. Since an actual call usually happens only after the definition of the derived class is available, there is no problem with a missing definition. At that point in time the derived class is not an incomplete type anymore.

“Ok, I get it. But how do we specify the return types of the subscript operators and begin() and end() functions?” The most convenient way to handle this is to use return type deduction. This is a perfect opportunity to use the decltype(auto) return type:

```
template< typename Derived >
struct DenseVector
{
    // ...

    decltype(auto) operator[]( size_t index )      { return derived()[index]; }
    decltype(auto) operator[]( size_t index ) const { return derived()[index]; }

    decltype(auto) begin()      { return derived().begin(); }
    decltype(auto) begin() const { return derived().begin(); }
    decltype(auto) end()      { return derived().end(); }
    decltype(auto) end()   const { return derived().end(); }
};
```

“Wouldn’t it be enough to just use auto? For instance, we could define the return types like this:”

```
template< typename Derived >
struct DenseVector
```

```
{ // ... Note: this doesn't always work, whereas decltype(auto) always works
    auto& operator[]( size_t index ) { return derived()[index]; }
    auto const& operator[]( size_t index ) const { return derived()[index]; }
    auto begin() { return derived().begin(); }
    auto begin() const { return derived().begin(); }
    auto end() { return derived().end(); }
    auto end() const { return derived().end(); }
};
```

It would be enough for this example, yes. However, as I keep emphasizing, code changes. Eventually, there may be another, deriving vector class that does not store its values and returns references to its values, but a vector class that produces values and returns by-value. And yes, this is easily conceivable: consider for instance a ZeroVector class, which represents the zero element for vectors. Such a vector would not store all of its elements, as this would be wasteful, but would likely be implemented as an empty class, which returns a zero by value every time an element is accessed. In that case, an auto& return type would be incorrect. Yes, the compiler would (hopefully) warn you about that. But you could evade the entire problem by just returning exactly what the deriving class returns. And that kind of return type is represented by the decltype(auto) return.

## Analyzing the Shortcomings of the CRTP Design Pattern

“Wow, this CRTP design pattern sounds amazing. So seriously, apart from these slightly-more-complex-than-usual implementation details, isn’t this the solution to all performance issues with virtual functions? And isn’t this the key, the holy grail for all inheritance-related problems?” I can understand the enthusiasm! At first sight CRTP most definitely looks like the ultimate solution for all kinds of inheritance hierarchies. However, unfortunately that is an illusion. Remember: every design pattern comes

with benefits, but unfortunately also with drawbacks. And there are several pretty limiting drawbacks to the CRTP design pattern.

The first and one of the most restricting drawbacks is the lack of a common base class. I will repeat this to emphasize the gravity of the repercussions: There is no common base class! Effectively, every single derived class has a different base class. For example, the `DynamicVector<T>` class has the `DenseVector<DynamicVector<T>>` base class. In contrast, the `StaticVector<T, Size>` class has the `DenseVector<StaticVector<T, Size>>` base class (see Figure 6-4). Thus whenever a common base class is required, a common abstraction that can be used for instance to store different types in a collection, the CRTP design pattern is not the right choice.

```
template< typename Derived >
class DenseVector
{
public:
  Derived& derived();
  const Derived& derived() const;
  // ...
};
```

High-level

Architectural
Boundary

Inversion of
dependencies

Low-level

```
template< typename T >
class DynamicVector
  : public DenseVector< DynamicVector<T> >
{
  // ...
};
```

```
template< typename T, size_t Size >
class StaticVector
  : public DenseVector< StaticVector<T,Size> >
{
  // ...
};
```

Figure 6-4. Dependency graph for the CRTP design pattern.
