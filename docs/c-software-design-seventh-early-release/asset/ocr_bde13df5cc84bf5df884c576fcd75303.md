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