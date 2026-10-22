Any function exclusively interested in JSON export can now specifically ask for that functionality:

```
void exportDocument( JSONExportable const& exportable )
{
    // ...
    exportable.exportToJSON( /* pass necessary arguments */ );
    // ...
}
```

Second, be aware that *External Polymorphism*, just as the *Adapter* design pattern, makes it very easy to wrap types that do not fulfill the semantic expectations. Similar to the duck typing example in “Guideline 24: Use Adapters to Standardize Interfaces”, where we pretended that a turkey is a duck, we also pretended that an `int` is a shape. All we had to do to fulfill the requirements was providing a free `draw()` function. Easy. Perhaps too easy. Therefore keep in mind, that the classes used to instantiate the `ShapeModel` class template (e.g. `Circle`, `Square`, etc.) *have* to adhere to the *LSP*. After all, the `ShapeModel` class acts just as a wrapper and passes on the requirements defined by the `ShapeConcept` class to the concrete shapes. Thus, the concrete shapes take the responsibility to properly implement the expected behavior (see “Guideline 6: Adhere to the Expected Behavior of Abstractions”). Any failure to completely fulfill the expectations may lead to (potentially subtle) misbehavior. Unfortunately, due to the fact that these requirements have been externalized, it is a little harder to communicate the expected behavior.

However, in the `int` example it was maybe our own fault to be honest. Perhaps the `ShapeConcept` base class does not really represent an abstraction of a shape. It is reasonable to argue that shapes are more than just drawing. Perhaps we should have named the abstraction `Drawable` and the *LSP* would have been satisfied. Perhaps not. So in the end, it all comes down to the choice of abstraction. Which brings us back to the title of Chapter 2: “The Art of Building Abstractions”. No, it is not easy, but perhaps these examples demonstrate that it is important. Very important. It may be the essence of software design.