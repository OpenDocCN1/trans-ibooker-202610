
{
    other.pimpl()->clone( pimpl() );
}

ShapeConstRef& operator=( ShapeConstRef const& other )
{
    // Copy-and-swap idiom
    ShapeConstRef copy( other );
    raw_.swap( copy.raw_ );
    return *this;
}

~ShapeConstRef()
{
    std::destroy_at( pimpl() );
    // or: pimpl()->~ShapeConcept();
}

// Move operations explicitly not declared  27

private:
    // ...
};
```

In addition to the templated ShapeConstRef constructor, ShapeConstRef offers two constructors to enable a conversion from Shape instances (27). While these are not strictly required, as we could also create an instance of a NonOwningShapeModel for a Shape, these constructors directly create a NonOwningShapeModel for the according, underlying shape type and thus shave off one indirection, which contributes to better performance. Note that in order to make these constructors work, ShapeConstRef needs to become a friend of the Shape class. Do not worry, though, as this is a good example for friendship: Shape and ShapeConstRef truly belong together, work hand in hand together, and are even provided in the same header file.

The last noteworthy detail is the fact that the two move operations are neither explicitly declared nor deleted (27). Since we have explicitly defined the two copy operations, the compiler does neither create nor delete the two move operations, thus they are gone. Completely gone in the sense that these two functions never participate in overload resolution. And yes, this is different from explicitly deleting them: if they were deleted, they would participate in overload resolution and if selected would result in a compilation error. But with these two functions gone, when you try to move a ShapeConstRef, the copy operations would be used instead, which however are cheap and efficient, since ShapeConstRef only represents a reference. Thus this class deliberately implements the Rule of 3.

We are almost finished. The last detail is one more addition, one more constructor in the Shape class:

```
//---- <Shape.h> ----------------

// ...

class Shape
{
public:
    // ...

    Shape( ShapeConstRef const& other )
        : pimpl_{ other.pimpl()->clone() }
    {}

private:
    // ...
}
```

Via this constructor, an instance of Shape creates a deep copy of the shape stored in the passed ShapeConstRef instance. Without this constructor, Shape would store a copy of the ShapeConstRef instance and thus act as a reference itself.

In summary, both non-owning implementations, the simple and the more complex one, give you all the design advantages of the Type Erasure design pattern, but at the same time pull you back into the realm of reference semantics, with all its deficiencies. Hence, utilize the strengths of this non-owning form of Type Erasure, but also be aware of the usual lifetime issues. Consider it on the same level as std::string_view and std::span. All of these serve as very useful tools for function arguments, but do not use them to store anything for a longer period, for instance in the form of a data member. The danger of lifetime related issues is just too high.

## GUIDELINE 34: BE AWARE OF THE SETUP COSTS OF OWNING TYPE ERASURE WRAPPERS

- Keep in mind that the setup of owning Type Erasure wrappers may involve copy operations and allocations;
- Be aware of non-owning Type Erasure, but also understand its reference semantics deficiencies;
- Prefer simple Type Erasure implementations, but know their limits;
- Prefer to use non-owning Type Erasure for function arguments, but not for data members or return types.

1 Yes, I consider the manual use of std::unique_ptr manual lifetime management. But of course it could be much worse if we would not reach for the power of RAII.

2 The term Type Erasure is heavily overloaded as it is used in different programming languages and for many different things. Even within the C++ community you hear the term begin used for various purposes: you might have heard it being used to denote void*, pointers-to-base, and std::variant. In the context of software design I consider this a very unfortunate issue. I will address this issue at the end of this guideline.

3 Sean Parent, “Inheritance Is The Base Class of Evil”, GoingNative 2013, YouTube.

4 Kevlin Henney, “Valued Conversions”, C++ Report July-August 2000, citeseer.

5 For an introduction to std::function see “Guideline 23: Prefer a Value-Based Implementation of Strategy and Command”.

6 The placement of ShapeConcept and OwningShapeModel in a namespace is purely an implementation detail of this example implementation. Still, as you will see in “Guideline 34: Be Aware of the Setup Costs of Owning Type Erasure Wrappers” this choice will come in pretty handy. Alternatively, these two classes can be implemented as nested classes. You will see examples for this in “Guideline 33: Know about the Optimization Potential of Type Erasure”.

7 Refer to “Guideline 31: Use External Polymorphism for Non-Intrusive Runtime Polymorphism” for the implementation based on std::function.

8 Many thanks to Arthur O’Dwyer for providing this example.

9 Again, please do not consider these performance numbers as the perfect truth. These are the performance results on my machine and my implementation. Your results will differ for sure. However, the take-away is that Type Erasure performs really well and might perform even better if we take the many optimization options into account (see “Guideline 33: Know about the Optimization Potential of Type Erasure”).

10 Eric Niebler on Twitter, June 19th, 2020 (Twitter).

11 For an introduction of std::variant, see “Guideline 17: Consider std::variant for Implementing Visitors”.

12 You should avoid going too deep, though, as you probably remember what happened to the dwarves of Moria who dug too deep…

13 Alternatively you could use an array of bytes, i.e. std::byte[Capacity], or std::aligned_storage. The advantage of std::array is that it enables you to copy the buffer (if that is applicable!).

14 Note that the choice for the default arguments for Capacity and Alignment are reasonable, but still arbitrary. You can of course use different defaults, that best fit the properties of the expected actual types.

15 You might not have seen a placement new before. If that is the case, rest assured that this form of new does not perform any memory allocation, but only calls a constructor to create an object at the specified address. The only syntactic difference is that you provide an additional pointer argument to new.

16 As a reminder, since you might not see this syntax often: the template keyword in the constructor is necessary because we are trying to call a function template on a dependent name (i.e. a name the meaning of which depends on a template parameter). Therefore you have to make it clear to the compiler that the following is the beginning of a template argument list and not a less-than comparison.

17 There is people that consider functions pointers to be the best feature of C++. In his lightning talk “The Very Best Feature of C++” (YouTube), James McNellis demonstrates their syntactic beauty and enormous flexibility. Please do not take this too serious, though, but rather as a humorous demonstration of a C++ imperfection.

18 At the time of writing there is an active proposal for a std::function_ref type, a non-owning version of std::function.

19 The term cv qualified refers to the const and volatile qualifiers.

20 For a reminder about lvalues and rvalues, refer to Nicolai Josuttis’ book on move semantics: “C++ Move Semantics - The Complete Guide”, NicoJosuttis, Aug. 2020, ISBN: 978-3967309003.

## Chapter 9. The Decorator Design Pattern

## A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 9th chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at sevans@oreilly.com.

This chapter is dedicated to another classic design pattern: the *Decorator* design pattern. Over the years, *Decorator* has proven to be one of the most useful design patterns when it comes to combining and reusing different implementations. So it does not come as a surprise that it is commonly used, even for one of the most impressive reworks of a C++ Standard Library feature. My primary objective in this chapter will be to give you a very good idea why, and when, *Decorator* is a great choice for designing software. Additionally, I will show you the modern, more value-based forms of *Decorator*.

In “Guideline 35: Use Decorators to Add Customization Hierarchically”, we will dive into the design aspects of the *Decorator* design pattern. You will see, when it is the right design choice and which benefits you are gaining by using it. Additionally, you will learn about differences compared to other design patterns and its potential shortcomings.
