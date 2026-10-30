
for the methods you end up using and more clearly communicates the “true” requirement of your code. As you can see, it can also significantly reduce the complexity of your bounds if the bounds on the underlying trait methods you want to call are complex.

## DERIVE TRAIT

While #[derive(Trait)] is extremely convenient, in the context of trait bounds, you should be aware of one subtlety around how it is often implemented. Many #[derive(Trait)] expansions desugar into impl Trait for Foo<T> where T: Trait. This is often what you want, but not always. For example, consider what happens if we try to derive Clone this way for Foo<T> and Foo contains an Arc<T>. Arc implements Clone regardless of whether T implements Clone, but due to the derived bounds, Foo will implement Clone only if T does! This isn’t usually too big of an issue, but it does add a bound where one isn’t needed. If we rename the type to Shared, the problem may become a little clearer. Imagine how confused a user that has a Shared<NotClone> will be when the compiler tells them that they cannot clone it! At the time of writing, this is how #[derive(Clone)] as provided by the standard library works, though this may change in the future.

Sometimes, you want bounds on associated types of types you’re generic over. As an example, consider the iterator method flatten, which takes an iterator that produces items that in turn implement Iterator and produces an iterator of the items of those inner iterators. The type it produces, Flatten, is generic over I, which is the type of the outer iterator. Flatten implements Iterator if I implements Iterator and the items yielded by I themselves implement IntoIterator. To enable you to write bounds like this, Rust lets you refer to associated types of a type using the syntax Type::AssocType. For example, we can refer to I’s Item type using I::Item. If a type has multiple associated types by the same name, such as if the trait that provides the associated type is itself generic (and therefore there are many implementations), you can disambiguate with the syntax <Type as Trait>::AssocType. Using this, you can write bounds not only for the outer iterator type but also for the item type of that outer iterator.

In code that uses generics extensively, you may find that you need to write a bound that talks about references to a type. This is normally fine, as you’ll tend to also have a generic lifetime parameter that you can use as the lifetime for these references. In some cases, however, you want the bound to say “this reference implements this trait for any lifetime.” This is exceedingly rare—at the time of writing, the standard library uses it in just three places—but it does happen. This type of bound is known as a higher-ranked trait bound, and it’s particularly useful in association with the Fn traits. For example, imagine you want to be generic over a function that takes a reference to a T and returns a reference to inside that T. If you write F: Fn(&T) -> &U, you need to provide a lifetime for those references, but you really want to say “any lifetime as long as the output is the same as the input.” Using a higher-ranked lifetime, you can write F: for<'a> Fn(&'a T) -> &'a U to say that for any lifetime 'a, the bound must hold. The Rust compiler is smart enough to automatically add the for when you write Fn bounds with references like this, which is part of why you so rarely need the explicit form, but it is worth knowing about.

To bring all of this together, consider the code in Listing 3-6, which can be used to implement Debug for any type that can be iterated over and whose elements are Debug.

```
impl Debug for AnyIterable
    where for<'a> &'a Self: IntoIterator,
        for<'a> <&'a Self as IntoIterator>::Item: Debug {
    fn fmt(&self, f: &mut Formatter) -> Result<(), Error> {
        f.debug_list().entries(self).finish()
    }
}
```

Listing 3-6: An excessively generic implementation of Debug for any iterable collection

You could copy-paste this implementation for pretty much any collection type and it would “just work.” Of course, you may want a smarter debug implementation, but this illustrates the power of trait bounds quite well.

## Marker Traits

Usually, we use traits to denote functionality that multiple types can support; a Hash type can be hashed by calling hash, a Clone type can be cloned by calling clone, and a Debug type can be formatted for debugging by calling fmt. But not all traits are functional in this way. Some traits, called marker traits, instead indicate a property of the implementing type. Marker traits have no methods or associated types and serve just to tell you that a particular type can or cannot be used in a certain way. For example, if a type implements the Send marker trait, it is safe to send across thread boundaries. If it does not implement this marker trait, it isn’t safe to send. There are no methods associated with this behavior; it’s just a fact about the type. The standard library has a number of these in the std::marker module, including Send, Sync, Copy, Sized, and Unpin. Most of these (all except Copy) are also auto traits; the compiler automatically implements them for types unless the type contains something that does not implement the marker trait.

Marker traits serve an important purpose in Rust: they allow you to write bounds that capture semantic requirements not directly expressed in the code. There is no call to send in code that requires that a type is Send. Instead, the code assumes that the given type is fine to use in a separate thread, and without marker traits the compiler would have no way of checking that assumption. It would be up to the programmer to remember the assumption and read the code very carefully, which we all know is not something we’d like to rely on. That path is riddled with data races, segfaults, and other runtime issues.

Similar to marker traits are *marker types*. These are unit types (like `struct MyMarker;`) that hold no data and have no methods. Marker types are useful for, well, marking a type as being in a particular state. They come in handy when you want to make it impossible for a user to misuse an API. For example, consider a type like `SshConnection`, which may or may not have been authenticated yet. You could add a generic type argument to `SshConnection` and then create two marker types: `Unauthenticated` and `Authenticated`. When the user first connects, they get `SshConnection<Unauthenticated>`. In its `impl` block, you provide only a single method: `connect`. `connect` returns a `SshConnection<Authenticated>`, and it’s only in that `impl` block that you provide the remaining methods for running commands and such. We will look at this pattern further in Chapter 4.

## Existential Types

In Rust you very rarely have to specify the types of variables you declare in the body of a function or the types of generic arguments to methods that you call. This is because of *type inference*, where the compiler decides what type to use based on what type the code the type appears in evaluates to. The compiler will usually infer types only for variables and for the arguments (and return types) of closures; top-level definitions like functions, types, traits, and trait implementation blocks all require that you explicitly name all types. There are a couple of reasons for this, but the primary one is that type inference is much easier when you have at least some known points to start the inference from. However, it’s not always easy, or even possible, to fully name a type! For example, if you return a closure from a function, or an async block from a trait method, its type does not have a name that you can type into your code.

To handle situations like this, Rust supports *existential types*. Chances are, you have already seen existential types in action. All functions marked as `async fn` or with a return type of `impl Trait` have an existential return type: the signature does not give the true type of the return value, just a hint that the function returns *some* type that implements some set of traits that the caller can rely on. And crucially, the caller can only rely on the return type implementing those traits, and nothing else.

> NOTE
> Technically, it isn’t strictly true that the caller relies on the return type and nothing else. The compiler will also propagate auto-traits like Send and Sync through impl Trait in return position. We’ll look at this more in the next chapter.

This behavior is what gives existential types their name: we are asserting that there exists some concrete type that matches the signature, and we leave it up to the compiler to find what that type is. The compiler will usually then go figure that out by applying type inference on the body of the function.
