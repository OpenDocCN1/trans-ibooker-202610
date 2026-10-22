every drawing strategy. And last but not least, you should keep in mind that class template usually completely reside in header files. You could therefore loose the opportunity to hide implementation details in a source file. As always, there is no perfect solution and the choice of the “right” solution depends on the actual context.

In summary, the *Strategy* design pattern is one of the most versatile examples in the catalogue of design patterns. You will find it useful in many situations, both in the realm of dynamic, as well as in the realm of static polymorphism. However, it is not the ultimate solution for every problem, be aware of its potential disadvantages.

# GUIDELINE 10: USE *STRATEGY* TO ISOLATE HOW THINGS ARE DONE

+   - Understand that inheritance is rarely the answer;
+   - Apply the *Strategy* design pattern with the intent to extract the implementation details of a cohesive set of functions;
+   - Implement one *Strategy* for each operation to avoid artificial coupling;
+   - Consider *Policy-Based Design* as the compile time form of the *Strategy* design pattern.

# Guideline 20: Favor Composition Over Inheritance

After the enormous surge of enthusiasm for object-oriented programming (OOP) in the 90s and early 2000s, OOP today is on the defensive. The voices that argue against OOP and highlight its disadvantages grow more and louder. And this is not limited to the C++ communities, but also in the communities of other programming languages. While OOP in its entirety