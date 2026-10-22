
## GUIDELINE 25: APPLY OBSERVERS AS AN ABSTRACT NOTIFICATION MECHANISM

- Apply the *Observer* design pattern with the intent to create a one-to-many relationship between a subject and its observers;
- Understand the trade-offs between *push observers* and *pull observers*;
- Utilize the advantages of a value semantics-based *Observer* implementation.

## Guideline 26: Use CRTP to Introduce Static Type Categories

C++ really has a lot to offer. It comes with lots of features, many syntactic curiosities and, in addition, a large number of amazing, utterly unpronounceable and (for the uninitiated) plainly cryptic acronyms: RAII, ADL, CTAD, SFINAE, NTTP, IFNDR, and SIOF. Oh, what fun! One of these cryptic acronyms is *CRTP*, short for the *Curiously Recurring Template Pattern.*¹² If you are now scratching your head because the name does not make any sense to you, do not worry: as so often in C++, the name was chosen pretty randomly, but has stuck and has never been reconsidered or changed afterwards. The pattern was named by James Coplien in the February 1995 issue of the C++ Report, after realizing that, curiously, this pattern was recurring in many different C++ code bases.¹³ And curiously, this pattern, although building on inheritance and (potentially) serving as an abstraction, does not exhibit the usual performance drawbacks of many other, classic design patterns. For that reason, *CRTP* is definitely worth a look, as it may become a valuable, or should I say *curious*, addition to your design pattern toolbox.

### A Motivation for CRTP

Performance is very important in C++. So important in fact, that in several contexts the performance overhead of using virtual functions is considered outright unacceptable. Therefore, in performance sensitive contexts, such as certain parts of computer games or high-frequency trading, no virtual functions are used. The same is true for high-performance computing (HPC): in HPC, any kind of conditional or indirection, and this includes virtual functions, is banned from the most performance critical parts, such as the innermost loops of compute kernels. Using them would incur too much of a performance overhead.

To give an example for how and why this matters, let us consider the following DynamicVector class template from a linear algebra (LA) library:

```
//---- <DynamicVector.h> ----------------

#include <numeric>
#include <iosfwd>
#include <iterator>
#include <vector>
// ...

template< typename T >
class DynamicVector
{
public:
    using value_type      = T;    ②
    using iterator        = typename std::vector<T>::iterator;
    using const_iterator  = typename std::vector<T>::const_iterator;

    // ... Constructors and special member functions

    size_t size() const;    ③

    T&       operator[]( size_t index );    ④
    T const& operator[]( size_t index ) const;

    iterator      begin();    ⑤
    const_iterator begin() const;
    iterator      end();
    const_iterator end() const;
```

```
// ... Many numeric functions
private:
    std::vector<T> values_;  ①
    // ...
};

template< typename T >
std::ostream& operator<<( std::ostream& os, DynamicVector const<T>& vector )  ⑥
{
    os << "(";
    for( auto const& element : vector ) {
        os << " " << element;
    }
    os << " )";

    return os;
}

template< typename T >
auto l2norm( DynamicVector const<T>& vector )  ⑦
{
    using std::begin, std::end;
    return std::sqrt( std::inner_product( begin(vector), end(vector)
        , begin(vector), T{} ) );
}

// ... Many more
```

Despite the name, DynamicVector does not represent a container, but a numerical vector for the purpose of LA computations. The Dynamic part of the name implies, that it allocates its elements of type T dynamically, in this example in the form of a std::vector ①. For that reason it is suited for large LA problems (definitely in the range of several million elements). Although this class may be loaded with many numerical operations, from an interface point of view you might indeed be tempted to call it a container: it provides the usual nested types (value_type, iterator, and const_iterator) ②, a size() function to query the current number of elements ③, subscript operators to access individual

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

```
// ...
};

template< typename T, size_t Size >
std::ostream& operator<<( std::ostream& os, StaticVector<T,Size> const& vector )
{
    os << "(";
    for( auto const& element : vector ) {
        os << " " << element;
    }
    os << " )";

    return os;
}

template< typename T, size_t Size >
auto l2norm( StaticVector<T,Size> const& vector )
{
    using std::begin, std::end;
    return std::sqrt( std::inner_product( begin(vector),
end(vector)
                                    , begin(vector), T{} ) );
}
```

“Isn’t this almost the same as the DynamicVector class?” you are wondering. Yes, these two classes are very similar indeed. The StaticVector class provides the same interface as the DynamicVector, such as the nested types value_type, iterator, and const_iterator (8), the size() member function (9), the subscript operators (10), and the begin() and end() functions (11). It also comes with an output operator (12) and a free l2norm() function (13). However, there is an important, performance-related difference between the two vector classes: as the Static in the name suggests, the StaticVector does not allocate its elements dynamically. Instead, it uses an in-class buffer to store its elements, for instance by means of a std::array (14). Thus in contrast to DynamicVector, the entire functionality of StaticVector is optimized for a small, fixed number of elements, such as 2D or 3D vectors.

“Ok, I understand that this is important for performance, but there still is a lot of code duplication, right?” Again, you are correct. If you take a close look at the associated output operator of the two vector classes, you will find that the implementation of these two functions is identical. This is deeply undesirable: in case anything changes, for instance the way vectors are formatted (and remember: change is the one constant in software development and needs to be expected; see “Guideline 2: Design for Change”), then you would have to make the change in many places, not just one. This is a violation of the Don’t Repeat Yourself (DRY) principle: it is easy to forget or to miss updating one of the many places, thus introducing an inconsistency or even a bug.

“But isn’t this duplication easily resolved by means of a slightly more general function template? For example, I can imagine the following output operator for all kinds of dense vectors:”

```
template< typename DenseVector >
std::ostream& operator<<( std::ostream& os, DenseVector const&
vector )
{
    // ... as before
}
```

Although this seems like an adequate solution, I would not accept this code in a pull request. This function template is indeed more general, but I would definitely not call it “slightly” more general: what you are suggesting is the most general output operator one could possibly write. Yes, the name of the function template may suggest that it is written for only dense vectors (including DynamicVector and StaticVector), but this function template will in fact accept any type: DynamicVector, StaticVector, std::vector, std::string, but also fundamental types such as int and double. It simply fails to specify any requirement or any kind of constraint. For that reason it violates the Core Guideline T.10:¹⁴
