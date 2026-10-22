*range*). The third argument represents the first element we need to copy to (the *output range*). A general expectation is, that the *output range* is big enough so all the elements from the *input range* can be copied to it.

But there are more expectations that are implicitly expressed via the names for the iterator types: `InputIt` and `OutputIt`. `InputIt` represents a type of *input iterator*. The C++ standard states all the expectations of such iterator types, as for instance the availability of an (in-)equality comparison, the ability to traverse a range with a prefix and postfix increment (i.e. `operator++()` and `operator++(int)`) and to access elements with the dereference operator (i.e. `operator*()`). `OutputIt`, on the other hand, represents a type of *output iterator*. Here, the C++ standard also explicitly states all expected operations.

`InputIt` and `OutputIt` may not be C++20 concepts, but still they represent the same idea: these named template parameters do not just give you an idea about what kind of type is required, but they express expected behavior. For instance, we expect that subsequent increments of `first` will eventually yield `last`. If any given concrete iterator type would not behave this way, `std::copy()` would not work as expected. This would be a violation of the expected behavior, and as such, a violation of the *LSP*⁹. Therefore both `InputIt` and `OutputIt` represent *LSP* abstractions.

Note that since concepts represent an *LSP* abstraction, i.e. a set of requirements and expectations, they are subject to the *Interface Segregation Principle (ISP)* as well (see “Guideline 3: Separate Interfaces to Avoid Artificial Coupling”). Just as you should separate concerns in the definition of requirements in the form of base classes (say “interface” classes), you should separate concerns when defining a concept. The standard library iterators do that by building on each other, thus allowing to select the desired level of requirements:

```
template< typename I >
concept input_or_output_iterator =
    /* ... */;

template< typename I >
```