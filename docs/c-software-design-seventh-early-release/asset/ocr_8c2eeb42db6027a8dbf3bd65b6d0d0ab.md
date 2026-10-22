use them to store anything for a longer period, for instance in the form of a data member. The danger of lifetime related issues is just too high.

# GUIDELINE 34: BE AWARE OF THE SETUP COSTS OF OWNING TYPE ERASURE WRAPPERS

+   - Keep in mind that the setup of owning Type Erasure wrappers may involve copy operations and allocations;
+   - Be aware of non-owning Type Erasure, but also understand its reference semantics deficiencies;
+   - Prefer simple Type Erasure implementations, but know their limits;
+   - Prefer to use non-owning Type Erasure for function arguments, but not for data members or return types.

1 Yes, I consider the manual use of std::unique_ptr manual lifetime management. But of course it could be much worse if we would not reach for the power of RAII.

2 The term Type Erasure is heavily overloaded as it is used in different programming languages and for many different things. Even within the C++ community you hear the term begin used for various purposes: you might have heard it being used to denote void*, pointers-to-base, and std::variant. In the context of software design I consider this a very unfortunate issue. I will address this issue at the end of this guideline.

3 Sean Parent, “Inheritance Is The Base Class of Evil”, GoingNative 2013, YouTube.

4 Kevlin Henney, “Valued Conversions”, C++ Report July-August 2000, citeseer.

5 For an introduction to std::function see “Guideline 23: Prefer a Value-Based Implementation of Strategy and Command”.

6 The placement of ShapeConcept and OwningShapeModel in a namespace is purely an implementation detail of this example implementation. Still, as you will see in “Guideline 34: Be Aware of the Setup Costs of Owning Type Erasure Wrappers” this choice will come in pretty handy. Alternatively, these two classes can be implemented as nested classes. You will see examples for this in “Guideline 33: Know about the Optimization Potential of Type Erasure”.