

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

除了模板化的 ShapeConstRef 构造函数之外，ShapeConstRef 还提供了两个构造函数以实现从 Shape 实例的转换（27）。严格来说这些并非必需，因为我们也可以为 Shape 创建一个 NonOwningShapeModel 实例，但这些构造函数直接为相应的底层形状类型创建 NonOwningShapeModel，从而省去了一层间接引用，这有助于提升性能。需要注意的是，为了使这些构造函数生效，ShapeConstRef 必须成为 Shape 类的友元。尽管如此也不必担心，因为这是一个友元使用的良好示例：Shape 和 ShapeConstRef 确实属于一起、手牵手地协同工作，并且甚至放在同一个头文件中提供。

最后一个值得注意的细节是，两个移动操作既未显式声明也未删除（27）。由于我们已显式定义了两个拷贝操作，编译器既不会创建也不会删除两个移动操作，因此它们消失了。完全消失的意思是，这两个函数永远不会参与重载决议。是的，这与显式删除它们不同：如果它们被删除，它们会参与重载决议，若被选中则会导致编译错误。但在没有这两个函数的情况下，当你尝试移动一个 ShapeConstRef 时，拷贝操作会被使用，而拷贝操作是廉价且高效的，因为 ShapeConstRef 只代表一个引用。因此该类刻意实现了 Rule of 3。

我们几乎完成了。最后一个细节是 Shape 类中的一个额外添加，一个额外的构造函数：

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

通过此构造函数，Shape 的实例会创建传递进来的 ShapeConstRef 实例中存储的形状的深拷贝。若没有此构造函数，Shape 将存储 ShapeConstRef 实例的拷贝，从而自身也充当引用。

总而言之，两种非拥有实现，简单和更复杂的，都能为你提供类型擦除设计模式的所有设计优势，但同时也将你拉回引用语义的范畴，并伴随其所有缺陷。因此，利用这种非拥有形式的 Type Erasure 的优势，但也要意识到通常的生命周期问题。将它视为与 std::string_view 和 std::span 同一级别。所有这些都可作为函数参数的非常有用的工具，但不要用它们来长期存储任何东西，例如以数据成员的形式。生命周期相关问题的危险性实在太高了。

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

