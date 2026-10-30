for those traits, and you should implement them only if you’re sure those semantics actually apply to your type.

Finally, for most types, it makes sense to implement the serde crate’s Serialize and Deserialize traits. These can be easily derived, and the serde_derive crate even comes with mechanisms for overwriting the serialization for just one field or enum variant. Since serde is a third-party crate, you may not wish to add a required dependency on it. Most libraries therefore choose to provide a serde feature that adds support for serde only when the user opts into it.

You might be wondering why I haven’t included the derivable trait Copy in this section. There are two things that set Copy apart from the other traits mentioned. The first is that users do not generally expect types to be Copy; quite to the contrary, they tend to expect that if they want two copies of something, they have to call clone. Copy changes the semantics of moving a value of the given type, which might surprise the user. This ties in to the second observation: it is very easy for a type to stop being Copy, because Copy types are highly restricted. A type that starts out simple can easily end up having to hold a String, or some other non-Copy type. Should that happen, and you have to remove the Copy implementation, that’s a backward incompatible change. It is much rarer that you will have to remove a Clone implementation, so that’s a less onerous promise to make.

# Ergonomic Trait Implementations

Rust does not automatically implement traits for references to types that implement traits. To phrase this a different way, you cannot generally call fn foo<T: Trait>(t: T) with a &Bar, even if Bar: Trait. This is because Trait may contain methods that take &mut self or self, which obviously cannot be called on &Bar. Nonetheless, this behavior might be very surprising to a user who sees that Trait has only &self methods!

For this reason, when you define a new trait, you’ll usually want to provide blanket implementations as appropriate for that trait for &T where T: Trait, &mut T where T: Trait, and Box<T> where T: Trait. You may be able to implement only some of these depending on what receivers the methods of Trait have. Many of the traits in the standard library have similar implementations, precisely because that leads to fewer surprises for the user.

Iterators are another case where you’ll often want to specifically add trait implementations on references to a type. For any type that can be iterated over, consider implementing IntoIterator for both &MyType and &mut MyType where applicable. This makes for loops work with borrowed instances of your type as well out of the box, just like users would expect.

# Wrapper Types

Rust does not have object inheritance in the classical sense. However, the Deref trait and its cousin AsRef both provide something a little like inheritance. These traits allow you to have a value of type T and call methods on some type U by calling them directly on the T-typed value if T: Deref<Target = U>. This feels like magic to the user, and is generally great.