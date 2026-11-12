create an example Camera that'll use filters to process the captured colors.

designing/fpij/Camera.java

```
@SuppressWarnings("unchecked")
public class Camera {
private Function<Color, Color> filter;

public Color capture(final Color inputColor) {
    final Color processedColor = filter.apply(inputColor);
    //... more processing of color...
    return processedColor;
}

//... other functions that use the filter ...
}
```

The Camera has a field for the filter, a reference to an instance of Function (much like the delegation example we saw earlier). This filter function can receive a Color and return a processed Color. Looking at what we have so far, the class may appear to use only one filter, but with a design tweak, we'll make it work with multiple filters.

To achieve this flexibility, we'll use a method that belongs to a special type called default methods. In addition to abstract methods, interfaces can have methods with implementation, marked as default. These methods are automatically added to the classes that implement the interfaces. This was done as a trick in Java to enhance existing classes with new methods without having to change each one of them. In addition, interfaces can have static methods.

In addition to the apply abstract method, the Function interface has a default method, andThen, to combine or chain multiple Functions.