**NOTE** Kotlin features the use of import aliasing using the `as` keyword. This allows for the renaming of objects, classes, methods and the like to be imported with a different name. In this case the `Cons` is already defined in `Stream`, so an import alias for the `List` data type’s `Cons` and `Nil` are imported as `ConsL` and `NilL` respectively. This is a handy trick for when namespace clashes occur without resorting to full package name qualifiers.

```
```kotlin
import chapter3.Cons as ConsL
import chapter3.Nil as NilL
```
```

The thing to notice in this trace is how the `filter` and `map` transformations are interleaved—the computation alternates between generating a single element of the output of `map`, and testing with `filter` to see if that element is divisible by 2 (adding it to the output list if it is). Note that we don’t fully instantiate the intermediate stream that results from the `map`. It’s exactly as if we had interleaved the logic using a special-purpose loop. For this reason, people sometimes describe streams as “first-class loops” whose logic can be combined using higher-order functions like `map` and `filter`.

Since intermediate streams aren’t instantiated, it’s easy to reuse existing combinators in novel ways without having to worry that we’re doing more processing of the stream than necessary. For example, we can reuse `filter` to define `find`, a method to return just the first element that matches if it exists. Even though `filter` transforms the whole stream, that transformation is done lazily, so `find` terminates as soon as a match is found:

```
```kotlin
fun <A> Stream<A>.find(p: (A) -> Boolean): Option<A> =
    filter(p).headOption()
```
```

The incremental nature of stream transformations also has important consequences for memory usage. Because intermediate streams aren’t generated, a transformation of the stream requires only enough working memory to store and transform the current element. For instance, in the transformation `Stream.of(1, 2, 3, 4).map { it + 10 }.filter { it % 2 == 0 }`, the garbage collector can reclaim the space allocated for the values 11 and 13 emitted by `map` as soon as `filter` determines they aren’t needed. Of course, this is a simple example; in other situations we might be dealing with larger numbers of elements, and the stream elements themselves could be large objects that retain significant amounts of memory. Being able to reclaim this memory as quickly as possible can cut down on the amount of memory required by your program as a whole.

We’ll have a lot more to say about defining memory-efficient streaming calculations, in particular calculations that require I/O, in part 4 of this book.