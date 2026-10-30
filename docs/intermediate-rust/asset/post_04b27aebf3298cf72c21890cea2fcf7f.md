
# 8

# MACROS

![](img/bbeab1f2edc3d45a3e6f7f6efbbb1747_104_0.png)

Macros are, in essence, a tool for making the compiler write code for you. You give the compiler a formula for generating code given some input parameters, and the compiler replaces every invocation of the macro with the result of running through the formula. You can think of macros as automatic code substitution where you get to define the rules for the substitution.

Rust’s macros come in many different shapes and sizes to make it easy to implement many different forms of code generation. The two primary types are *declarative* macros and *procedural* macros, and we will explore both of them in this chapter. We’ll also look at some of the ways macros can come in handy in your everyday coding, and some of the pitfalls that arise with more advanced use. Some familiarity with macros is assumed, so before reading this chapter I recommend that you read the chapter on macros in the Rust language reference (https://doc.rust-lang.org/reference/macros.html).

Programmers coming from C-based languages may be used to the unholy land of C and C++ where you can use #define to change each true to false, or to remove all occurrences of the else keyword. If that’s the case for you, you’ll need to disassociate macros from a feeling of doing something “bad.” Macros in Rust are far from the Wild West of C macros. They follow (mostly) well-defined rules, and are fairly misuse-resistant.

# Declarative Macros

Declarative macros are those defined using the macro_rules! syntax, which lets you conveniently define function-like macros without having to resort to writing a dedicated crate for the purpose (as you do with procedural macros). Once you’ve defined a declarative macro, you can invoke it using the name of the macro followed by an exclamation mark. I like to think of this kind of macro as a sort of compiler-assisted search and replace: it does the job for many regular, well-structured transformation tasks, and for eliminating repetitive boilerplate. In your experience with Rust up until this point, most of the macros you have recognized as macros are likely to have been declarative macros. Note, however, that not all function-like macros are declarative macros; macro_rules! itself is one example of this, and format_args! is another. The ! suffix merely indicates to the compiler that the macro invocation will be replaced with different source code at compile time.

> NOTE  
> Since Rust’s parser specifically recognizes and parses macro invocations annotated with !, you can only use them in places where the parser allows them. They work in most places you’d expect, like in expression position or in an impl block, but not everywhere. For example, you cannot (at the time of writing) invoke a function-like macro where an identifier or match arm is expected.

It may not be immediately obvious why declarative macros are called declarative. After all, don’t you “declare” everything in your program? In this context, declarative refers to the fact that you don’t say how the macro’s inputs should be translated into the output, just that you want the output to look like A when the input is B. You declare that it shall be so, and the compiler figures out all the parsing rewiring that has to happen to make your declaration reality. This makes declarative macros concise and expressive, though it also has a tendency to make them rather cryptic since you have a limited language to express your declarations in.

# When to Use Them

Declarative macros are primarily useful when you find yourself writing the same code over and over, and you’d like to, well, not do that. They’re best suited for fairly mechanical replacements—if you’re aiming to do fancy code transformations or lots of code generation, procedural macros are likely a better fit.

I most frequently make use of declarative macros in cases where I find myself writing repetitive and structurally similar code, such as in tests and trait implementations. For tests, I often want to run the same test multiple times but with slightly different configurations. I might have something like what is shown in Listing 8-1.

```rust
fn test_inner<T>(init: T, frobnify: bool) { ... }
#[test]
fn test_1u8_frobnified() {
    test_inner(1u8, true);
}
// ...
#[test]
fn test_i128_not_frobnified() {
    test_inner(1i128, false);
}
```

*Listing 8-1: Repetitive testing code*

While this works, it’s too verbose, too repetitive, and too prone to manual error. With macros we can do much better, as shown in Listing 8-2.

```rust
macro_rules! test_battery {
    ($($t:ty as $name:ident),*) => {
        $(
            mod $name {
                #[test]
                fn frobnified() { test_inner::<$t>(1, true) }
                #[test]
                fn unfrobnified() { test_inner::<$t>(1, false) }
            }
        )*
    }
}
test_battery! {
    u8 as u8_tests,
    // ...
    i128 as i128_tests
}
```

*Listing 8-2: Making a macro repeat for you*

This macro expands each comma-separated directive into its own module that then contains two tests, one that calls `test_inner` with true, and one with false. While the macro definition isn’t trivial, it makes adding more tests much easier. Each type is one line in the `test_battery!` invocation, and the macro will take care of generating tests for both true and false arguments. We could also have it generate tests for different values for `init`. We’ve now significantly reduced the likelihood that we’ll forget to test a particular configuration!

The story for trait implementations is similar. If you define your own trait, you’ll often want to implement that trait for a number of types in the standard library, even if those implementations are trivial. Let’s imagine you invented the Clone trait and want to implement it for all the Copy types in the standard library. Instead of manually writing out an implementation for each one, you can use a macro like the one in Listing 8-3.

```rust
macro_rules! clone_from_copy {
    ($( $t:ty ),*) => {
        $(impl Clone for $t {
            fn clone(&self) -> Self { *self }
        })*
    }
}
clone_from_copy![bool, f32, f64, u8, i8, /* ... */];
```

Listing 8-3: Using a macro to implement a trait for many similar types in one fell swoop

Here, we generate an implementation of Clone for each provided type whose body just uses * to Copy out of &self. You may wonder why we don’t add a blanket implementation of Clone for T where T: Copy. We could do that, but a big reason not to is that it would force types in other crates to also use that same implementation of Clone for their own types that happen to be Copy. There is an experimental compiler feature called specialization that could offer a workaround, but at the time of writing the stabilization of that feature that is still some way off. So, for the time being, we’re better off enumerating the types specifically. This pattern also extends beyond simple forwarding implementations: you could easily adapt the example in Listing 8-3 to implement an AddOne trait to all integer types, for example!

> NOTE  
> If you ever find yourself wondering if you should use generics or a declarative macro, you should use generics. Generics are generally more ergonomic than macros, and integrate much better with other constructs in the language. If you want a rule of thumb: if your code changes based on type, use generics; otherwise, use macros.

# How They Work

Every programming language has a grammar that dictates how the individual characters that make up the source code can be turned into tokens. Tokens are the lowest-level building blocks of a language, such as numbers, punctuation characters, string and character literals, and identifiers; at this level, there’s no distinction between language keywords and variable names. For example, the text (value + 4) would be represented by the five-token sequence (, value, +, 4, ) in Rust-like grammar. The process of turning text into tokens also provides a layer of abstraction between the rest of the compiler and the gnarly low-level details of parsing text. For example, in the token representation there is no notion of whitespace, and /*"foo"*/ and "/*foo*/" have distinct representations (the former is no token, the latter is a string literal token with the content /*foo*/).

Once the source code has been turned into a sequence of tokens, the compiler walks that sequence and assigns syntactic meaning to the tokens. For example, ()-delimited tokens make up a group, ! tokens denote macro invocations, and so on. This is the process of parsing, which ultimately produces an abstract syntax tree (AST) that describes the structure represented by the
