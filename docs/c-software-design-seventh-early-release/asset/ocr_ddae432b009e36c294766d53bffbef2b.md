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