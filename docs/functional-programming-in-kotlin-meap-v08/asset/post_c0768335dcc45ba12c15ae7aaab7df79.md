
## 3.1 Defining functional data structures

A functional data structure is operated on using only pure functions. As you may recall from chapter 1, a pure function must not change data in place or perform other side effects. Therefore, functional data structures are by definition immutable. An empty list should be as eternal and immutable as the integer values 3 or 4. And just as evaluating 3 + 4 results in a new number 7 without modifying either 3 or 4, concatenating two lists together (the syntax for this is a + b for two lists a and b) yields a new list and leaves the two inputs unmodified.

Doesn’t this mean we end up doing a lot of extra copying of the data? Perhaps surprisingly, the answer is no, and we’ll talk about exactly why that is later in this section. But first let’s examine what’s probably the most ubiquitous functional data structure, the singly linked list. It serves as a good example due to its simplicity, making it easy to reason about and understand the underlying principles of immutable data structures. Listing 3.1 introduces some new syntax and concepts that we’ll talk through in detail.

### Listing 3.1 Definition of the singly linked list data structure

```
sealed class List<out A> {
    // helper functions
}

object Nil : List<Nothing>()

data class Cons<out A>(
    val head: A,
    val tail: List<A>
) : List<A>()
```

- ① Sealed definition of data type
- ② The Nil implementation of List
- ③ The Cons implementation of List

Let’s look first at the definition of the data type, which begins with the keywords sealed class . Usually, we introduce a data type with the class keyword. Here we’re declaring a class, called List, with no instance methods on it. Adding sealed in front of the class declaration means that all implementations must be declared in this file. A sealed class is also abstract by default, so can not be instantiated by itself. There are two implementations, or data constructors, of List declared next, to represent the two possible forms a List can take. As the figure shows, a List can be empty, denoted by the data constructor Nil, or it can be nonempty, denoted by the data constructor Cons (traditionally short for construct). A nonempty list consists of an initial element, head, followed by a List (possibly empty) of remaining elements (the tail).

![](img/255e7d4fd2ecc6516db2405b06be1018_35_0.png)

Figure 3.1 The singly linked list, each tail links to the next list element.

Just as functions can be polymorphic, data types can be as well, and by adding the type parameter <out A> after sealed class List and then using that A parameter inside of the Cons data constructor, we declare the List data type to be polymorphic in the type of elements it contains, which means we can use this same definition for a list of Int elements (denoted List<Int>), Double elements (denoted List<Double>), String elements (List<String>), and so on (the out indicates that the type parameter A is covariant—see sidebar “More about variance” for more information).

A data constructor declaration gives us a function to construct that form of the data type. Here are a few examples:

```
val ex1: List<Double> = Nil
val ex2: List<Int> = Cons(1, Nil)
val ex3: List<String> = Cons("a", Cons("b", Nil))
```

Using object Nil, you can write Nil to construct an empty List, and the data class Cons lets you write Cons(1, Nil), Cons("a", Cons("b", Nil)) and so on to build singly linked lists of arbitrary lengths. Note that because List is parameterized on a type, A, these are polymorphic functions that can be instantiated with different types for A. Here, ex2 instantiates the A type parameter to Int, while ex3 instantiates it to String. The ex1 example is interesting—Nil is being instantiated with type List<Double>, which is allowed because the empty list contains no elements and can be considered a list of whatever type we want!

## SIDEBAR
### More about variance

In the declaration `class List<out A>`, the `out` in front of the type parameter `A` is a variance annotation that signals that `A` is a covariant or “positive” parameter of `List`. This means that, for instance, `List<Dog>` is considered a subtype of `List<Animal>`, assuming `Dog` is a subtype of `Animal`. (More generally, for all types `X` and `Y`, if `X` is a subtype of `Y`, then `List<X>` is a subtype of `List<Y>`). We could leave out the `out` in front of the `A`, which would make `List` invariant in that type parameter.

But notice now that `Nil` extends `List<Nothing>`. `Nothing` is a subtype of all types, which means that in conjunction with the variance annotation, `Nil` can be considered a `List<Int>`, a `List<Double>`, and so on, exactly as we want.

These concerns about variance aren’t very important for the present discussion and are more of an artifact of how Kotlin encodes data constructors via subtyping, so don’t worry if this is not completely clear right now. It’s certainly possible to write code without using variance annotations at all, and function signatures are sometimes simpler (whereas type inference often gets worse). We’ll use variance annotations throughout this book where it’s convenient to do so, but you should feel free to experiment with both approaches.

If you would like to learn more about generics, including covariance and contravariance in Kotlin, feel free to read the Kotlin documentation at bit.ly/2Op7bmh.

Many other languages provide the feature of *pattern matching* to work with such data types, as in the functions `sum` and `product`. We’ll examine how we achieve this with the `when` expression in more detail next.

## 3.2 Working with functional data structures

Up to this point we have focused our attention on the definition of the most basic functional data structure, the singly linked list. Having this definition isn’t of much use unless we actually start *doing* something with it. In this section, you will learn to apply the technique of *matching*, in order to interpret and process the `List` that was defined in section 3.1.

### Listing 3.2 Companion object inside sealed definition of List data type

```
sealed class List<out A> {
    companion object {
        fun <A> of(vararg aa: A): List<A> {
            val tail = aa.sliceArray(1 until aa.size)
            return if (aa.isEmpty()) Nil else Cons(aa[0], of(*tail))
        }

        fun sum(ints: List<Int>): Int =
            when (ints) {
                is Nil -> 0
                is Cons -> ints.head + sum(ints.tail)
            }

        fun product(doubles: List<Double>): Double =
            when (doubles) {
                is Nil -> 1.0
                is Cons ->
                    if (doubles.head == 0.0) 0.0
                    else doubles.head * product(doubles.tail)
            }
    }
}
```

- ① Definition of List data structure
- ② Companion object containing functions
- ③ Factory helper function

In order to add some behaviour to the List type, a companion object is added to the body of its definition. Any functions defined within the companion object block can be called like you would a static method in Java. For instance, the of method can be used in the following way to construct a new List from the parameters passed in:

```
>>> List.of(1, 2)
res0: chapter3.List<kotlin.Int> = Cons(head=1, tail=Cons(head=2, tail=Nil))
```

This method accepts a parameter qualified by a vararg keyword. This means that the function is variadic in nature, meaning that we can pass in an arbitrary amount of parameters of the same type in place of that parameter. These values are then bound to the parameter as an array of that type, and can subsequently be accessed in the method body. We don’t need to know much more about this, although we explain it in detail in the sidebar.

## SIDEBAR
### Variadic functions in Kotlin

The `of` function in the `List` object is a factory method for creating new `List` instances. This method is also a *variadic function*, meaning it accepts zero or more arguments of type `A`. If no argument is provided, it will result in a `Nil` instance of `List`. If arguments *are* provided, the method will return a `Cons` representing those values.

```
fun <A> of(vararg aa: A): List<A> {
    val tail = aa.sliceArray(1 until aa.size)
    return if (aa.isEmpty()) Nil else Cons(aa[0], List.of(*tail))
}
```

For data types, it is a common idiom to have a variadic `of` method in the companion object to conveniently construct instances of the data type. By calling this function `of` and placing it in the companion object, we can invoke it with syntax like `List.of(1, 2, 3, 4)` or `List.of("hi", "bye")`, with as many values as we want separated by commas.

In the example, the parameter `aa` to the method is marked with a preceding `vararg` keyword, and will subsequently be available as type `Array<out A>` despite having the declared type of `A`. In this case we use the `sliceArray` method of the `Array` type to extract the `tail` as a new `Array`.

It is also possible to pass an array *into* a method as a variadic parameter by using a prefixed *spread operator*, `*`. We have done so with the recursive call to `of` with `*tail` in our example.

Although the details of working with arrays are unimportant in the context of this discussion, more information on this topic can be found in the Kotlin documentation at bit.ly/2VkgpSa.

Let’s look in detail at the functions, `sum` and `product`, which we placed in the companion object. Both these definitions make use of a matching technique using the `when` expression:

### Listing 3.3 Function definitions in the List companion object

```
fun sum(ints: List<Int>): Int =
    when (ints) {
        is Nil -> 0
        is Cons -> ints.head + sum(ints.tail)
    }

fun product(doubles: List<Double>): Double =
    when (doubles) {
        is Nil -> 1.0
        is Cons ->
            if (doubles.head == 0.0) 0.0
            else doubles.head * product(doubles.tail)
    }
```
