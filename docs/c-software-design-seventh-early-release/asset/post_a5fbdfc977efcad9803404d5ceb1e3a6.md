
reason why I said “consider using the name”. You should decide on a case to case basis.

## Examples from the Standard Library

One useful application of the Adapter design pattern is to standardize the interface of different kinds of containers. Let us assume the following Stack base class:

```
//---- <Stack.h> ----------------

template< typename T >
class Stack
{
public:
    virtual ~Stack() = default;
    virtual T& top() = 0;
    virtual bool empty() const = 0;
    virtual size_t size() const = 0;
    virtual void push( T const& value ) = 0;
    virtual void pop() = 0;
};
```

This Stack class provides the necessary interface to access the top element of the stack, to check if the stack is empty, to query the size of the stack, to push an element onto the stack, and to remove the top element of the stack. This base class can now be used to implement different Adapters for various data structures, for instance std::vector:

```
//---- <VectorStack.h> ----------------

#include <Stack.h>

template< typename T >
class VectorStack : public Stack<T>
{
public:
    T& top() override { return vec_.back(); }
    bool empty() const override { return vec_.empty(); }
    size_t size() const override { return vec_.size(); }
    void push( T const& value ) override { vec_.push_back(value);
}
    void pop() override { vec_.pop_back(); }
    private:
        std::vector<T> vec_;
};
```

“Do you seriously suggest implementing a stack by means of an abstract base class? Aren’t you worried about the performance implications? For every use of a member function you have to pay with a virtual function call!” you worry. No, of course I do not suggest that. Obviously you are correct and I completely agree with you: from a C++ perspective, this kind of container feels strange and very inefficient. Because of efficiency, we usually realize the same idea via class templates. This is the approach taken by the C++ standard library in form of the three STL classes called Container adaptors: std::stack, std::queue, and std::priority_queue:

```
template< typename T
    , typename Container = std::deque<T> >
class stack;
```

```
template< typename T
    , typename Container = std::deque<T> >
class queue;
```

```
template< typename T
    , typename Container = std::vector<T>
    , typename Compare = std::less<typename Container::value_type> >
class priority_queue;
```

The purpose of these three class templates is to adapt the interface of a given Container type to a special purpose. For instance, the purpose of the std::stack class template is to adapt the interface of a container to the stack operations top(), empty(), size(), push(), emplace(), pop(), and swap(). By default, you are able to use the three available sequence containers std::vector, std::list, and std::deque. For any other container type, you are able to specialize the std::stack class template.

“This feels so much more familiar”, you state, visibly relieved. Again, I absolutely agree. I also consider the standard library approach the more suitable solution for the purpose of containers. But it is still interesting to compare the two approaches. While there are many technical differences between the Stack base class and the std::stack class template, the purpose and semantics of these two approaches are remarkably similar: both provide the ability to adapt any data structure to a given stack interface. And both serve as a variation point, allowing you to non-intrusively add new Adapters without having to modify existing code.

## Comparison between Adapter and Strategy

“The three STL classes seem to fulfill the intent of Adapters, but isn’t this the same way of configuring behavior as in the Strategy design pattern? Isn’t this similar to std::unique_ptr and its deleter?”, you are asking. And yes, you are correct. From a structural point of view, the Strategy and Adapter design patterns are very similar. However, as explained in “Guideline 11: Understand the Purpose of Design Patterns”, the structure of design patterns may be similar or even the same, but the intent is a different one. In this context, the Container parameter does not only specify a single aspect of the behavior, but most of the behavior or even all of it. The class templates merely act as a wrapper around the functionality of the given type, they mainly adapt the interface. So the primary focus of an Adapter is to standardize interfaces and to integrate incompatible functionality into an existing set of conventions, while on the other hand the primary focus of the Strategy design pattern is to enable the configuration of behavior from the outside, building on, and providing, an expected interface. Also, in case of an Adapter there is no need to reconfigure the behavior at any time.

## Function Adapters

Another example for the Adapter design pattern are the Standard Library’s free functions begin() and end(). “Are you serious?”, you ask surprised. “You claim that free functions serve as an example for the

Adapter design pattern? Isn’t this a job for classes?" Well, not necessarily. The purpose of the free begin() and end() functions is to adapt the iterator interface of any type to the expected STL iterator interface. Thus it maps from an available set of functions to an expected set of functions and as such serves the same purpose as any other Adapter. The major difference is, that in contrast to object adapters or class adapters, which are based on either inheritance (runtime polymorphism) or templates (compile time polymorphism), begin() and end() draw their power from function overloading, which is the second major compile time polymorphism mechanism in C++. Still, some form of abstraction is at play.

> **NOTE**
> 
> Remember that all kinds of abstraction represent a set of requirements and thus have to adhere to the Liskov Substitution Principle (LSP). This is also true for overload sets; see "Guideline 8: Understand the Semantic Requirements of Overload Sets".

Consider the following function template:

```
template< typename Range >
void traverseRange( Range const& range )
{
    for( auto&& element : range ) {
        // ...
    }
}
```

In the traverseRange() function, we iterate through all the elements contained in the given range by means of a range-based for loop. The traversal happens via iterators, that the compiler acquires by means of the free begin() and end() functions. Hence the above for loop is equivalent to the following form of for:

```
template< typename Range >
void traverseRange( Range const& range )
{
    {
```

```
using std::begin;
using std::end;

auto first( begin(range) );
auto last ( end(range) );
for( ; first!=last; ++first ) {
    auto&& element = *first;
    // ...
}
}
```

Obviously the range-based for loop is much more convenient to use. However, underneath the surface, the compiler generates code based on the free begin() and end() functions. Note the two using declarations in the beginning: the purpose of these two is to enable Argument-Dependent Lookup (ADL) for the given type of range. ADL is the mechanism that makes sure the “correct” begin() and end() functions are called, even if they are overloads that reside in a user-specific namespace. That means, that you have the opportunity to overload begin() and end() for any type and map the expected interface to a different, special purpose set of functions.

This kind of function adapter has been called a shim by Matthew Wilson in 2004. One valuable property of this technique is, that it is completely non-intrusive: it is possible to add a free function to any type, even to types that you could never adapt, such as types provided by third-party libraries. Hence any generic code written in terms of shims, gives you the enormous power to adapt virtually any type to the expected interface. Thus, you can imagine that shims or function adapters are the backbone of generic programming.

## Analyzing the Shortcomings of the Adapter Design Pattern

Despite the value of the Adapter design pattern, there is one issue with this design pattern, that I should explicitly point out. Consider the following example, which I adopted from Eric Freeman and Elisabeth Robson:
