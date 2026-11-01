For example, if we have a structure full of integers and want to calculate their sum, we can use foldRight:

```
ints.foldRight(0) { a, b -> a + b }
```

Looking at this code snippet, we shouldn’t have to care about the type of ints at all. It could be a Vector, a Stream, a List, or anything at all with a foldRight method for that matter. We can capture this commonality in the following interface for all these container types:

```
interface Foldable<F> {
    fun <A, B> foldRight(fa: Kind<F, A>, z: B, f: (A, B) -> B): B
    fun <A, B> foldLeft(fa: Kind<F, A>, z: B, f: (B, A) -> B): B
    fun <A, B> foldMap(fa: Kind<F, A>, m: Monoid<B>, f: (A) -> B): B
    fun <A> concatenate(fa: Kind<F, A>, m: Monoid<A>): A =
        foldLeft(fa, m.nil, m::combine)
}
```

① The interface declares type F that represents any container

② The Kind<F, A> represents the kind of F<A>

The Foldable interface declares a generic type of F, which could represent any container such as Option, List or Stream. We also see something new by way of a type called Kind<F, A>, which represents the F<A>. We can’t express multiple levels of generics in Kotlin type declarations, so Arrow provides us with Kind to declare that the kind of F is an outer container for inner elements of type A. Just like functions that take other functions as arguments are called higher-order functions, something like Foldable is a higher-order type constructor or a higher-kinded type.