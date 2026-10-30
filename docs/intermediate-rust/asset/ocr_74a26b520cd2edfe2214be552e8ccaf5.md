lifetime parameters if you have a type that contains multiple references, and its methods return references that should be tied to the lifetime of only one of those references.

Consider the type in Listing 2-10, which gives you an iterator over parts of a string separated by a particular other string.

```
struct StrSplit<'s, 'p> {
    delimiter: &'p str,
    document: &'s str,
}
impl<'s, 'p> Iterator for StrSplit<'s, 'p> {
    type Output = &'s str;
    fn next(&self) -> Option<Self::Output> {
        todo!()
    }
}
fn str_before(s: &str, c: char) -> Option<&str> {
    StrSplit { document: s, delimiter: &c.to_string() }.next()
}
```

Listing 2-10: A type that needs to be generic over multiple lifetimes

When you construct this type, you have to give the delimiter and document to search, both of which are references to string values. When you ask for the next string, you get a reference into the document. Consider what would happen if you used a single lifetime in this type. The values yielded by the iterator would be tied to the lifetime of the document and the delimiter. This would make str_before impossible to write: the return type would have a lifetime associated with a variable local to the function—the String produced by to_string—and the borrow checker would reject the code.

# Lifetime Variance

Variance is a concept that programmers are often exposed to but rarely know the name of because it’s mostly invisible. At a glance, variance describes what types are subtypes of other types and when a subtype can be used in place of a supertype (and vice versa). Broadly speaking, a type A is a subtype of another type B if A is at least as useful as B. Variance is the reason why, in Java, you can pass a Turtle to a function that accepts an Animal if Turtle is a subtype of Animal, or why, in Rust, you can pass a &'static str to a function that accepts a &'a str.

While variance usually hides out of sight, it comes up often enough that we need to have a working knowledge of it. Turtle is a subtype of Animal because a Turtle is more “useful” than some unspecified Animal—a Turtle can do anything an Animal can do, and likely more. Similarly, 'static is a subtype of 'a because a 'static lives at least as long as any 'a and so is more useful. Or, more generally, if 'b: 'a ('b outlives 'a), then 'b is a subtype of 'a. This is obviously not the formal definition, but it gets close enough to be of practical use.

All types have a variance, which defines what other similar types can be used in that type’s place. There are three kinds of variance: covariant,