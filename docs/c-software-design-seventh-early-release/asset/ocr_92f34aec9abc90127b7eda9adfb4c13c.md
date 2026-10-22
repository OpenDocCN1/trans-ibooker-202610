```
'CustomType' */
}

} // namespace custom
```

If swap() is used correctly, this custom function will perform a special kind of swap operation on two instances of CustomType:39

```
template< typename T >
void some_function( T& value )
{
    // ...
    T tmp( /*...*/ );

    using std::swap;        // Enable the compiler to consider
    std::swap for the       // subsequent call
    swap( tmp, value );     // Swap the two values; thanks to the
    unqualified call        // and thanks to ADL this would call
    'custom::swap()'
    // ...                    // in case 'T' is 'CustomType'
}
```

Obviously, std::swap() is designed as a customization point, allowing you to plug in new, custom types and behavior. The same is true of all algorithms in the standard library. Consider for instance std::find() and std::find_if():

```
template< typename InputIt, typename T >
constexpr InputIt find( InputIt first, InputIt last, T const&
value );
```

```
template< typename InputIt, typename UnaryPredicate >
constexpr InputIt find_if( InputIt first, InputIt last,
UnaryPredicate p );
```

By means of the template parameters, and implicitly the according concepts, std::find() and std::find_if() (just as all other algorithms) enable you to use your own (iterator) types to perform a search. In addition, std::find_if() allows you to customize the way the