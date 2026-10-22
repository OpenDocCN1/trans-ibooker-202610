21 To get an overview of C++ performance aspects in general and on performance related issues with inheritance hierarchies in particular, you might consider to refer to Kurt Guntheroth’s book “Optimized C++”, O’Reilly Media, Apr. 2016, ISBN: 978-1491922064.

22 A possible solution for that is to employ techniques from data-oriented design; see Richard Fabian, “Data-oriented design: software engineering for limited resources and short schedules”, Richard Fabian, September 2018, ISBN: 978-1916478701.

23 Mark my choice of words: “we might get the following output”. Indeed, we might get this output, but also something else. It depends, as we have inadvertently entered the realm of undefined behavior. Therefore this output is my best guess, not a guarantee.

24 Now not only your manicurist, but also your hairdresser has work to do…

25 More gray hairs, more work for your hairdresser.

26 I should explicitly point out that the notion of a “deep copy” depends on the type T of elements in the vector: if T performs a deep copy, then so does the std::vector, but if T performs a shallow copy, then semantically std::vector also performs a shallow copy.

27 The best and most complete introduction to move semantics is Nicolai Josuttis’ book on the subject: “C++ Move Semantics - The Complete Guide”, NicoJosuttis, Aug. 2020, ISBN: 978-3967309003.

28 See Patrice Roy’s CppCon 2016 talk “The Exception Situation” for a similar example and discussion (YouTube).

29 Yet this is exactly the approach taken by the std::atoi() function…

30 In his standard proposal P0709, Herb Sutter explains that 52% of C++ developers have no or limited access to exceptions.

31 The experienced C++ developer also knows that C++23 will bless us with a very similar type called std::expected. In a few years, this might be the appropriate way to write the to_int() function.

32 From a functional programming point of view, std::optional represents a monad. You will find much more valuable information on monads and functional programming in general in Ivan Čukić’s book on “Functional Programming in C++”, Manning, Nov. 2018, ISBN: 978-1617293818.

33 In this example, the std::function object performs a deep copy, but generally speaking, std::function copies the contained callable according to its copy semantics (“deep” or “shallow”). std::function has no way of forcing a deep copy.

34 This implementation detail is explained thoroughly by Nicolai Josuttis in this CppCon 2017 talk “The Nightmare of Move Semantics for Trivial Classes” (YouTube).

35 One more example for the KISS principle.

36 A discussion about the reasons for the performance deficiencies of some std::function implementations would go beyond the scope and purpose of this book. Still, please keep this detail in mind for performance critical sections of your code.