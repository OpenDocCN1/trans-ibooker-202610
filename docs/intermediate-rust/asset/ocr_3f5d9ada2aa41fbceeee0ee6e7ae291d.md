possible. It does this by giving every type an alignment that’s computed based on the types that it contains. Built-in values are usually aligned to their size, so a u8 is byte-aligned, a u16 is 2-byte-aligned, a u32 is 4-byte-aligned, and a u64 is 8-byte-aligned. Complex types—types that contain other types—are typically assigned the largest alignment of any type they contain. For example, a type that contains a u8, a u16, and a u32 will be 4-byte-aligned because of the u32.

# Layout

Now that you know about alignment, we can explore how the compiler decides on the in-memory representation, known as the layout, of a type. By default, as you’ll see shortly, the Rust compiler gives very few guarantees about how it lays out types, which makes for a poor starting point for understanding the underlying principles. Luckily, Rust provides a repr attribute you can add to your type definitions to request a particular in-memory representation for that type. The most common one you will see, if you see one at all, is repr(C). As the name suggests, it lays out the type in a way that is compatible with how a C or C++ compiler would lay out the same type. This is helpful when writing Rust code that interfaces with other languages using the foreign-function interface, which we’ll talk about in Chapter 12, as Rust will generate a layout that matches the expectations of the other language’s compiler. Since the C layout is predictable and not subject to change, repr(C) is also useful in unsafe contexts if you’re working with raw pointers into the type, or if you need to cast between two different types that you know have the same fields. And, of course, it is perfect for taking our first steps into layout algorithms.

> Another useful representation is repr(transparent), which can be used only on types with a single field and which guarantees that the layout of the outer type is exactly the same as that of the inner type. This comes in handy in combination with the “newtype” pattern, where you may want to operate on the in-memory representations of some struct A and struct NewA(A) as if they were the same. Without repr(transparent), the Rust compiler does not guarantee that they will have the same layout.

So, let’s look how the compiler would lay out a particular type with repr(C): the Foo type in Listing 3-1. How do you think the compiler would lay this out in memory?

```
#[repr(C)]
struct Foo {
    tiny: bool,
    normal: u32,
    small: u8,
    long: u64,
    short: u16,
}
```

Listing 3-1: Alignment affects layout.