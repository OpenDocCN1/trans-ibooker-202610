With this filter combination, the input color goes through a series of transformations or filtering. First, it passes through the bright filter, which brightens the shades, and then it goes through the dark filter, which makes the colors darker again, as we can see from the output.

```
with brighter & darker filter: java.awt.Color[r=178,g=99,b=178]
```

Adding more than two filters is no different; we simply pass more comma-separated filters, either as method references or as lambda expressions.

We designed object chaining and implemented the decorator pattern without having to create a hierarchy of classes. The magic happens in just a couple of lines of code within the setFilters method. We made good use of the JDK Function interface here. We saw lambda expressions and method references really shine in this example.

We ran into another Java feature in this section: the default methods. Let's take a closer look at them next.