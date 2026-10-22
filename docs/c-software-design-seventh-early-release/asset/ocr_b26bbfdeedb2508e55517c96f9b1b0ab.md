elements by index (one for non-const and one for const vectors) ④, and begin() and end() functions to iterate over the elements ⑤. Apart from the member functions, it also provides an output operator ⑥ and, to show at least one LA operation, a function to compute the vector’s Euclidean norm (often also called the L2 norm, because it approximates the L2 norm for discrete vectors) ⑦.

The DynamicVector is not the only vector class, though. In our LA library, you will also find the following StaticVector class:

```
//---- <StaticVector.h> ----------------

#include <array>
#include <numeric>
#include <iosfwd>
#include <iterator>
// ...

template< typename T, size_t Size >
class StaticVector
{
public:
    using value_type      = T;    ⑧
    using iterator        = typename std::array<T,Size>::iterator;
    using const_iterator = typename std::array<T,Size>::const_iterator;

    // ... Constructors and special member functions

    size_t size() const;    ⑨

    T&       operator[]( size_t index );    ⑩
    T const& operator[]( size_t index ) const;

    iterator      begin();    ⑪
    const_iterator begin() const;
    iterator      end();
    const_iterator end() const;

    // ... Many numeric functions

private:
    std::array<T,Size> values_;    ⑫
```