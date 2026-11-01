
## 10 Monoids

### This chapter covers:
- An introduction to purely algebraic structures
- Learning about monoids and the laws they impose
- The connection between monoids and fold operations
- Using balanced folds to perform parallel computations in chunks
- Higher-kinded types and their application to foldable data structures
- Composing monoids to perform complex calculations from simpler parts

By the end of part 2, we were getting comfortable with considering data types in terms of their *algebras*. In particular, we were concerned with the operations they support and the laws that govern those operations. By now you will have noticed that the algebras of very different data types tend to share certain patterns in common. In this chapter, we’ll begin identifying these patterns and taking advantage of them.

This chapter will be our first introduction to *purely algebraic structures*. As an example, we’ll begin by considering a simple structure known as the *monoid*, which is defined *only by its algebra*. The name *monoid* might sound intimidating at first, but it is merely a mathematical term that in category theory refers to a *category with one object*. Other than satisfying the same laws, instances of the monoid interface may have little or nothing to do with one another. Nonetheless, we’ll see how this algebraic structure is often all we need to write useful, polymorphic functions.

We choose to start with monoids because they’re simple, ubiquitous, and useful. Monoids appear often in everyday programming whether we’re aware of it or not. Working with lists, concatenating strings, or accumulating the results of a loop can often be phrased in terms of the monoid. In situations like this the use of *monoid instances* are employed as concrete implementations of this algebraic structure. We will begin with defining some monoid instances for combining integers, booleans and `Options`. We will then also see how monoid instances are a perfect fit for implementing `fold` operations on lists.

The chapter later culminates in how monoids can be used in two real-world situations: they facilitate parallelization by giving us the freedom to break problems into chunks that can be computed in parallel; they can also be composed to assemble complex calculations from simpler parts.

### 10.1 What is a monoid?

Grasping algebraic structures such as the monoid might seem like a daunting task, but when approaching it from a purely algebraic perspective we come to realize how simple it actually is. Rather than trying to explain it in words, we will first explore the concept by way of example.

Let’s begin by considering the algebra of string concatenation. We can add "foo" + "bar" to get "foobar". In addition to this, the empty string is known as an *identity element* for that operation. That is, if we say (s + "") or ("" + s), the result is always s for any value of s. Furthermore, if we combine three strings by saying (r + s + t), the operation is *associative*. By this we mean that it doesn’t matter whether we parenthesize it: (r + s) + t or r + (s + t).

The exact same rules govern integer addition. It is associative since (x + y) + z is always equal to x + (y + z). It has an identity element of 0 which does nothing when added to another integer. Ditto for multiplication, which works in the same way but has an identity element of 1.

The Boolean operators && and || are likewise associative, and they have identity elements true and false, respectively.

These are just a few simple examples, but when you go looking, algebras like this can be found everywhere where laws apply. The term for this particular kind of algebra is *monoid*, and the laws of associativity and identity are collectively called the *monoid laws*. A monoid consists of the following:

- Some type `A`
- An associative binary operation `combine`, that takes two values of type `A` and combines them into one: `combine(combine(x,y), z) == combine(x, combine(y,z))` for any choice of `x: A, y: A, z: A`

![](img/255e7d4fd2ecc6516db2405b06be1018_214_0.png)

Figure 10.1 The law of associativity expressed in terms of combine for monoids

- The value nil: A, that is an identity for that operation: combine(x, nil) == x and combine(nil, x) == x for any x: A

![](img/255e7d4fd2ecc6516db2405b06be1018_214_1.png)

Figure 10.2 The law of identity expressed in terms of combine for monoids

We can express this in terms of a Kotlin interface:

### Listing 10.1 A monoid expressed as a Kotlin interface

```
interface Monoid<A> {
    fun combine(a1: A, a2: A): A
    val nil: A
}
```

① Satisfies the law of associativity, combine(combine(x, y), z) == combine(x, combine(y, z))

② Satisfies the law of identity, combine(x, nil) == x and combine(nil, x) == x

An example instance of this interface is the String monoid:

```
val stringMonoid = object : Monoid<String> {
    override fun combine(a1: String, a2: String): String = a1 + a2
    override val nil: String = ""
}
```

List concatenation also forms a monoid, this method is able to generate a monoid for any type A:

```
fun <A> listMonoid(): Monoid<List<A>> = object : Monoid<List<A>> {
    override fun combine(a1: List<A>, a2: List<A>): List<A> = a1 + a2
    override val nil: List<A> = emptyList()
}
```

### SIDEBAR The purely abstract nature of an algebraic structure

Notice that other than satisfying the monoid laws, the various Monoid instances don’t have much to do with each other. The answer to the question “What is a monoid?” is simply that a monoid is a type, together with the monoid operations and a set of laws. A monoid is the algebra, and nothing more. Of course, you may build some other intuition by considering the various concrete instances, but this intuition isn’t necessarily correct and nothing guarantees that all monoids you encounter will match your intuition!

### EXERCISE 10.1

Give Monoid instances for integer addition and multiplication, as well as for Boolean operators.

```
val intAddition: Monoid<Int> = TODO()

val intMultiplication: Monoid<Int> = TODO()

val booleanOr: Monoid<Boolean> = TODO()

val booleanAnd: Monoid<Boolean> = TODO()
```

### EXERCISE 10.2

Give a Monoid instance for combining Option values.

```
fun <A> optionMonoid(): Monoid<Option<A>> = TODO()

fun <A> dual(m: Monoid<A>): Monoid<A> = TODO()
```

### EXERCISE 10.3

A function having the same argument and return type is sometimes called an endofunction.⁴⁹ Write a monoid for endofunctions.

```
fun <A> endoMonoid(): Monoid<(A) -> A> = TODO()
```

### EXERCISE 10.4

Use the property-based testing framework we developed in chapter 8 to implement properties for the monoid laws of associativity and identity. Use your properties to test some of the monoids we've written so far.

```
fun <A> monoidLaws(m: Monoid<A>, gen: Gen<A>): Prop = TODO()
```

### Talking about monoids

Programmers and mathematicians disagree about terminology when they talk about a type being a monoid versus having a monoid instance.

As a programmer, it is tempting to think of the instance of type Monoid<A> as being a monoid. But that isn't really true. The monoid is actually both things—the type together with the instance satisfying the laws. It's more accurate to say that the type A forms a monoid under the operations defined by the Monoid<A> instance. Put in a different way, we might say that "type A is a monoid," or even that "type A is monoidal." In any case, the Monoid<A> instance is simply evidence of this fact.

This is much the same as saying that the page or screen you're reading "forms a rectangle" or "is rectangular." It's less accurate to say that it "is a rectangle" (although that still makes sense), but to say that it "has a rectangle" would be strange.

Just what is a monoid, then? It's simply a type A and an implementation of Monoid<A> that satisfies the laws. Stated otherwise, a monoid is a type together with a binary operation ( combine) over that type, satisfying associativity and having an identity element (nil).

What does this buy us? Just like any abstraction, a monoid is useful to the extent that we can write useful generic code assuming only the capabilities provided by the abstraction. Can we write any interesting programs, knowing nothing about a type other than that it forms a monoid? Absolutely! Let's look at some examples.

### 10.2 Folding lists with monoids

Monoids have an intimate connection with lists. If we recall the various fold operations that were defined on the List type in chapter 3, we see that two parameters were always present: a zero value initializer, and a function that combined two values into an accumulated result. All of this was done in context of a single type, namely that of the initializer value.

Let's take a closer look at the signatures of foldLeft and foldRight on List to confirm the above observation.
