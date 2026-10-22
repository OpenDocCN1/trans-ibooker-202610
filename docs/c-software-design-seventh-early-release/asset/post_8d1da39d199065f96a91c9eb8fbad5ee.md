
The Shape class template is rewritten to accept a StoragePolicy. Via this policy you would be able to specify from outside how the class should acquire memory. And of course you would perfectly adhere to SRP and OCP. One such storage policy could be the DynamicStorage policy class:

```
#include <utility>

struct DynamicStorage
{
    template< typename T, typename... Args >
    T* create( Args&&... args ) const
    {
        return new T( std::forward<Args>( args )... );
    }

    template< typename T >
    void destroy( T* ptr ) const noexcept
    {
        delete ptr;
    }
};
```

As the name suggests, DynamicPolicy would acquire memory dynamically, for instance via new. Alternatively, if you have stronger requirements, you could build on std::aligned_alloc() or similar functionality to provide dynamic memory with a specified alignment. Similarly to DynamicStorage, you could provide a InClassStorage policy:

```
#include <array>
#include <cstddef>
#include <memory>
#include <utility>

template< size_t Capacity, size_t Alignment >
struct InClassStorage
{
    template< typename T, typename... Args >
    T* create( Args&&... args ) const
    {
        static_assert( sizeof(T) <= Capacity, "The given type is too large" );
        static_assert( alignof(T) <= Alignment, "The given type is misaligned" );

        T* memory = const_cast<T*>(reinterpret_cast<T const*> (buffer_.data()));
        return std::construct_at( memory, std::forward<Args>( args )... );

        // or:
        // void* const memory = static_cast<void*>(buffer_.data());
        // return ::new (memory) T( std::forward<Args>( args )...
);
    }

    template< typename T >
    void destroy( T* ptr ) const noexcept
    {
        std::destroy_at(ptr);
        // or: ptr->~T();
    }

    alignas(Alignment) std::array<std::byte,Capacity> buffer_;
};
```

All of these policy classes provide the same interface: a create() function to instantiate an object of type T, and a destroy() function to do whatever is necessary to clean up. This interface is used by the Shape class to trigger construction and destruction, for instance in its templated constructor (⑧)¹⁶ and in the destructor (⑨).

```
template< typename StoragePolicy >
class Shape
{
public:
    template< typename ShapeT >
    Shape( ShapeT shape )
    {
        using Model = OwningModel<ShapeT>;
        pimpl_ = policy_.template create<Model>( std::move(shape) )
⑧
    }

    ~Shape() { policy_.destroy( pimpl_ ); } ⑨
```

```
// ... All other member functions, in particular the
//     special members functions, are not shown

private:
    // ...
    [[no_unique_address]] StoragePolicy policy_{};  ⓐ
    Concept* pimpl_{};
};
```

The last detail, that should not be left unnoticed, is the data members (ⓐ): the Shape class now stores an instance of the given StoragePolicy and, do not be alarmed, a raw pointer to its Concept. Indeed, there is no need to store a std::unique_ptr anymore, since we are manually destroying the object in our own destructor again. You might also notice the [[no_unique_address]] attribute on the storage policy. This C++20 feature gives you the opportunity to save the memory for the storage policy. In case the policy is empty, the compiler is now allowed to not reserve any memory for the data member. Without this attribute, it would be necessary to reserve at least a single byte for policy_, which due to alignment restrictions is likely more.

In summary, SBO is an effective and one of the most interesting optimizations for a Type Erasure implementation. For that reason, many standard types, such as std::function and std::any, use some form of SBO. Unfortunately, the C++ standard library specification does not require the use of SBO. This is why you can only hope that SBO is used, you cannot count on it. However, because performance is so important and because SBO plays such a decisive role, there is already proposals out there that suggest to also standardize the types inplace_function and inplace_any. Time will tell if these find their way into the Standard Library.

## Manual Implementation of Function Dispatch

“Wow, this will prove useful. Is there anything else I can do to improve the performance of my Type Erasure implementation?” you are asking. Oh yes,

you can do more. There is a second potential performance optimization. This time we try to improve the performance of the virtual functions. And yes, I am talking about the virtual functions, that are introduced by the external inheritance hierarchy, i.e. by means of the *External Polymorphism* design pattern.

“How should we be able to optimize the performance of virtual functions? Isn’t this something that is completely up to the compiler?” Absolutely, you are correct. However, I am not talking about fiddling with backend, compiler-specific implementation details, but about replacing the virtual functions with something more efficient. And that is indeed possible. Remember that a virtual function is nothing but a function pointer that is stored inside a virtual function table. Every type with at least one virtual function has such a virtual function table. However, there is only one virtual function table for each type. In other words, this table is not stored inside every instance. So in order to connect the virtual function table with every instance of that type, the class stores an additional, hidden data member, which we commonly call the `vptr` and which is a raw pointer to the virtual function table.

When you call a virtual function, you first go through the `vptr` to fetch the virtual function table. Once you are there, you can grab the according function pointer from the virtual function table and call it. Therefore, in total, a virtual function call entails two indirections: first the `vptr`, and second the pointer to the actual function. For that reason, roughly speaking, a virtual function call is twice as expensive as a regular, non-inlined function call.

These two indirections provide us with the opportunity for optimization: we can in fact reduce the number of indirections to just one. In order to achieve that we will employ an optimization strategy that fairly often works: we trade space for speed. What we will do is to implement the virtual dispatch manually by storing the virtual function pointers inside the `Shape` class. The following code snippet already gives you a pretty good idea of the details:

```
//---- <Shape.h> ----------------

#include <cstddef>
#include <memory>

class Shape
{
public:
    // ...

private:
    // ...

    template< typename ShapeT
            , typename DrawStrategy >
    struct OwningModel ⑮
    {
        OwningModel( ShapeT value, DrawStrategy drawer )
            : shape_( std::move(value) )
            , drawer_( std::move(drawer) )
        {}

        ShapeT shape_;
        DrawStrategy drawer_;
    };

    using DestroyOperation = void(void*);      ⑯
    using DrawOperation    = void(void*);      ⑰
    using CloneOperation   = void*(void*);     ⑱

    std::unique_ptr<void,DestroyOperation*> pimpl_;  ⑲
    DrawOperation* draw_ { nullptr };            ⑳
    CloneOperation* clone_ { nullptr };          ㉑
};
```

Since we are replacing all virtual functions, even the virtual destructor, there is no need for a Concept base class anymore. So consequently, the external hierarchy is reduced to just the OwningModel class template (⑮), which still acts as storage for a specific kind of shape (i.e. ShapeT) and DrawStrategy. Still, it meets the same fate: all virtual functions are removed. The only remaining details are the constructor and the data members.

The virtual functions are replaced by manual function pointers. Since the syntax for function pointers is not the most pleasant to use, we add a couple of function type aliases for our convenience:¹⁷ `DestroyOperation` represents the former virtual destructor (⑲), `DrawOperation` represents the former virtual `draw()` function (⑰), and `CloneOperation` represents the former virtual `clone()` function (⑲).
`DestroyOperation` is used to configure the `Deleter` of the `pimpl_` data member (⑲) (and yes, as such it acts as a *Strategy*). The latter two, i.e. `DrawOperation` and `CloneOperation`, are used for the two additional function pointer data members `draw_` and `clone_` (²⁰ and ²¹).

“Oh no, void*s! Isn’t that an archaic and super dangerous way of doing things?”, you gasp. Ok, I admit that without explanation it looks very suspicious. However, stay with me, I promise that everything will be perfectly fine and type-safe. The key to making this work now lies in the initialization of these function pointers. They are initialized in the templated constructor of the `Shape` class:

```
//---- <Shape.h> -------------
// ...
class Shape
{
public:
    template< typename ShapeT
            , typename DrawStrategy >
    Shape( ShapeT shape, DrawStrategy drawer )
        : pimpl_( ②
            new OwningModel<ShapeT,DrawStrategy>(
            std::move(shape)
            std::move(drawer) )
            , []( void* shapeBytes ){ ²³
                using Model = OwningModel<ShapeT,DrawStrategy>;
                auto* const model = static_cast<Model*>
                (shapeBytes); ²⁴
                delete model;  ²⁵
            } )
        , draw_( ²⁶
```
