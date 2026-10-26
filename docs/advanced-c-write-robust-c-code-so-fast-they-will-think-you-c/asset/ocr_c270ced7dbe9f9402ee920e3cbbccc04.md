+   Identifiers ........................................................................ 255
+   Names .............................................................................. 255
+   Name lookup ....................................................................... 256
+   Argument-Dependent Lookup ......................................................... 256
+   Caveat Emptor ...................................................................... 258
+   Exercise 5: Implementing Templates to Prevent ADL Issues ......................... 260
+   Implicit Conversion ................................................................ 262
+   Explicit – Preventing Implicit Conversion ............................................ 262
+   Contextual Conversion ............................................................... 265
+   Exercise 6: Implicit and Explicit Conversions ........................................ 266
+   Activity 2: Implementing classes for Date Calculations .............................. 270
+   Summary ............................................................................. 271

# Chapter 4: Separation of Concerns - Software Architecture, Functions, and Variadic Templates ........................................ 273

+   Introduction ....................................................................... 274
+     The Pointer to Implementation (PIMPL) Idiom ....................................... 274
+     Logical and Physical Dependencies .................................................. 274
+     The Pointer to Implementation (PIMPL) Idiom ....................................... 276
+     Advantages and Disadvantages of PIMPL ............................................. 280
+     Implementing PIMPL with unique_ptr<> .............................................. 280
+     unique_ptr<> PIMPL Special Functions .............................................. 283
+     Exercise 1: Implementing a Kitchen with unique_ptr<> .............................. 286
+   Function Objects and Lambda Expressions ............................................. 290
+     Function Pointers .................................................................. 290
+     What is a Function Object? .......................................................... 294
+     Exercise 2: Implementing function objects .......................................... 297
+   std::function<> template ............................................................. 299