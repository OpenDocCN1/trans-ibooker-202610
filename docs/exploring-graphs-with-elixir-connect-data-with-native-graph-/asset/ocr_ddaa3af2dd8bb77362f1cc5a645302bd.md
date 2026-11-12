is given, then the reduce method (we saw this method in Reducing a Collection to a Single Value) will return an Optional empty. In that case, we provide a dummy filter as an argument to the orElse method, and it simply returns the color that the filter would receive for processing. If we provide filters to the setFilters method, the filter field will refer to the first filter—an instance of Function<Color, Color>—that’s at the head of a chain of filters.

We provided a lambda expression as a parameter to the orElse method of the Optional that the reduce method returned. The Function interface has an identity static method that does the same operation as the lambda expression we wrote. Instead of creating our own lambda expression, we can use a reference to that method instead. To do so, we need to change

```
.orElse(color -> color);
```

to

```
.orElseGet(Function::identity);
```

In addition to the setFilters method, we have a constructor that simply sets the filter to the dummy filter I mentioned previously.

Our design of the camera with filters is complete, so let’s try it out. We’ll use it with no filters first, but we need a Camera instance to start. Let’s create one and assign it to a local variable camera.

```
designing/fpij/Camera.java
final Camera camera = new Camera();
final Consumer<String> printCaptured = (filterInfo) ->
System.out.println(String.format("with %s: %s", filterInfo,
camera.capture(new Color(200, 100, 200))));
```