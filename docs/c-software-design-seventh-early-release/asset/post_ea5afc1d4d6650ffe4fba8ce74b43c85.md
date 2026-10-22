
*Specify concepts for all template arguments.*

—C++ Core Guideline T.10

While this output operator will work for all dense vectors and also sequence containers, you would get a compilation error for all types that do not provide the expected interface. Or even worse: you might subtly violate the implicit requirements and expectations and by that the *Liskov Substitution Principle (LSP)* (see “Guideline 6: Adhere to the Expected Behavior of Abstractions”). Of course you would not do this consciously, but likely accidentally: this output operator is a perfect match for any type and might be used even though you do not expect it. Therefore this function template would be a very unfortunate addition to the output operator overload set. What we totally need is a new set of types, a new type category…

> “Isn’t this what base classes are for? Couldn’t we just formulate a DenseVector base class that defines the expected interface for all dense vectors? Consider the following sketch of a DenseVector base class:”

```
template< typename T >  // Type of the elements
class DenseVector
{
public:
    virtual ~DenseVector() = default;

    virtual size_t size() const = 0;

    virtual T&       operator[]( size_t index ) = 0;
    virtual T const& operator[]( size_t index ) const = 0;

    // ...
};

template< typename T >
std::ostream& operator<<( std::ostream& os, DenseVector<T> const&
vector )
{
    // ... as before
}
```

“This should work, right? I’m just not sure how to declare the begin() and end() functions, as I don’t know how to abstract from different

iterator types, such as std::vector<T>::iterator and std::array<T>::iterator.” I also have a feeling that this could be a problem and I admit that I also do not have a quick solution for that. But there is something far more concerning: with this base class we would turn all our member functions into virtual member functions. That would include the begin() and end() functions, but most importantly the two subscript operators. The consequences would be significant: With every access to an element of the vector, we would now have to call a virtual function. Every single access! Therefore, with this base class, we could wave high performance goodbye.

Still, the general idea of building an abstraction by means of a base class is good. We just have to do it differently. This is where we should take a closer look at the Curiously Recurring Template Pattern (CRTP).

## The CRTP Design Pattern Explained

The CRTP design pattern is building on the common idea of creating an abstraction by means of a base class. But instead of establishing a runtime relationship between base and derived classes via virtual functions, it creates a compile time relationship:

> THE CRTP DESIGN PATTERN
> Intent: “Define a compile time abstraction for a family of related types.”

The compile time relationship between the DenseVector base class and the DynamicVector derived class is created by upgrading the base class to a class template:

```
//---- <DenseVector.h> ----------------

template< typename Derived > 
struct DenseVector
{
```

```
// ...
    size_t size() const { return static_cast<Derived const&>
(*this).size(); } ⑰
    // ...
};
```

```
//---- <DynamicVector.h> ----------------

template< typename T >
class DynamicVector : public DenseVector<DynamicVector<T>> ⑯
{
public:
    // ...
    size_t size() const;  ⑱
    // ...
};
```

The curious detail about CRTP is that the new template parameter of the DenseVector base class represents the type of the associated derived class (⑩). Derived classes, for instance the DynamicVector, are expected to provide their own type to instantiate the base class (⑬).

“Wow, wait a second: is that even possible?” you are asking. It is. In order to instantiate a template you do not need the complete definition of a type. It is sufficient to use an incomplete type. Such an incomplete type is available after the compiler has seen the class DynamicVector declaration. In essence, this piece of syntax works as a forward declaration. Therefore the DynamicVector class can indeed use itself as a template argument to the DenseVector base class.

Of course you can name the template parameter of the base class as you want to (e.g. simply T), but as discussed in “Guideline 14: Use a Design Pattern’s Name to Communicate Intent” it helps to communicate intent by using the name of the design pattern or names commonly used for a pattern. For that reason you could name the parameter CRTP, which nicely communicates the pattern, but unfortunately only to the initiated. Everyone else will be puzzled by the acronym. Therefore the template parameter is often called Derived, which perfectly expresses its purpose and communicates its intent: it represents the type of the derived class.

Via this template parameter, the base class is now aware of the actual type of the derived type. While it itself still represents an abstraction and the common interface for all dense vectors, it is now able to access and call the concrete implementation in the derived type. This happens for instance in the size() member function (⑰): the DenseVector uses a static_cast to convert itself into a reference to the derived class and calls the size() function on that. What at first glance may look like a recursive function call (calling the size() function within the size() function), is in fact a call of the size() member function in the derived class (⑱).

“So this is the compile time relationship you were taking about. The base class represents an abstraction from concrete derived types and implementation details, but still knows exactly were the implementation details are. So we really do not need any virtual function.” Correct. With CRTP we are now able to implement a common interface and forward every call to the derived class by simply performing a static_cast. And there is no performance penalty for doing this. In fact, the base class function is very likely to be inlined and, if the DenseVector is the only or first base class, the static_cast will not even result in a single assembly instruction. It merely tells the compiler to treat the object as an object of the derived type.

In order to provide a clean CRTP base class, we should update a couple of details, though:

```
//---- <DenseVector.h> ----------------

template< typename Derived >
struct DenseVector
{
protected:
    ~DenseVector() = default;    ⑲

public:
    Derived&       derived()      { return static_cast<Derived&>( *this ); }  ⑳
    Derived const& derived() const { return static_cast<Derived
const&> ( *this ); }

    size_t size() const { return derived().size(); }

    // ...
};
```

Since we want to avoid any virtual functions, we are also not interested in a virtual destructor. Therefore we implement the destructor as a non-virtual function in the protected section of the class ⑩. This perfectly adheres to Core Guideline C.35:

> A base class destructor should be either public and virtual, or protected and non-virtual.

—Core Guideline C.35

Keep in mind, though, that this definition of the destructor keeps the compiler from generating the two move operations. Since a CRTP base class is usually an empty base class with nothing to move, this is not a problem, but still, always be mindful about the Rule of 5.

We should also avoid using a static_cast in every single member function of the base class. Although it would be correct, any cast should be considered suspicious and therefore casts should be minimized.¹⁵ For that reason, we add the two derived() member functions, which perform the cast and can be used in the other member functions ⑳. This resulting code does not just look cleaner and adheres to the DRY principle, it also looks far less suspicious.

Equipped with the derived() functions, we can now go ahead and define the subscript operators and the begin() and end() functions:

```
template< typename Derived >
struct DenseVector
{
    // ...

    ??? operator[]( size_t index )      { return derived()
[index]; }
    ??? operator[]( size_t index ) const { return derived()
```
