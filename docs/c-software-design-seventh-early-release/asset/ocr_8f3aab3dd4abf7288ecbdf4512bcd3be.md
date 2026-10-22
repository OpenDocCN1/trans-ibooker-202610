Again, we first create an empty vector of shapes (this time a vector of `std::unique_ptrs` of `ShapeConcept`) ⑧, before we add three shapes. Within the calls to `std::make_unique()`, we instantiate the `ShapeModel` class for `Circle` and `Square` (called `CircleModel` ⑩ and `SquareModel` ⑩ to improve readability) and pass the necessary details (i.e. the concrete shape and the according `OpenGLDrawStrategy`). After that, we are able to draw all shapes in the desired way.

Altogether, this approach gives you a lot of awesome advantages:

+   - Due to separating concerns and extracting the polymorphic behavior from the shape types, you remove all dependencies on graphics libraries, etc. This creates a very loose coupling and beautifully adheres to the *SRP*;
+   - The shape types become simpler and non-polymorphic;
+   - You are able to easily add new kinds of shapes. These might even be third-party types, as you are no longer required to intrusively inherit from a `Shape` base class or create an *Adapter* (see “Guideline 24: Use Adapters to Standardize Interfaces”). Thus, you perfectly adhere to the *OCP*;
+   - You significantly reduce the usual inheritance-related boilerplate code and implement it in exactly one place, which very nicely follows the *DRY* principle;
+   - Since the `ShapeConcept` and `ShapeModel` class belong together and only together form the abstraction, it is much easier to adhere to the *DIP*;
+   - Due to reducing the number of indirections by exploiting the available class template, you can improve performance.

There is one more advantage, which I consider to be the most impressive benefit of the *External Polymorphism* design pattern: you can, non-intrusively, equip any type with polymorphic behavior. Really, *any* type,