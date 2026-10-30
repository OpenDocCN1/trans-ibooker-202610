
heavy dependencies. For example, the syn crate, which provides a parser for Rust token streams that makes the experience of writing procedural macros much easier, can take tens of seconds to compile with all features enabled. This can (and should) be mitigated by disabling features you do not need, and compiling your procedural macros in debug mode rather than release mode. Code often compiles several times faster in debug mode, and for most procedural macros you won’t even notice the difference in execution time.

The second reason is that procedural macros make it easy to generate a lot of code without realizing it. While the macro saves you from having to actually type the generated code, it does not save the compiler from having to parse, compile, and optimize it. As you use more procedural macros, that generated boilerplate adds up, and it can bloat your compile times.

It’s worth pointing out that the actual execution time of procedural macros is rarely a factor in overall compile time. While the compiler has to wait for the procedural macro to do its thing before it can continue, in practice most procedural macros do not do any heavy computation. That said, if your procedural macro is particularly involved, you may end up with your compiles spending a significant chunk of execution time on your procedural macro code, which is worth keeping an eye out for!

## So You Think You Want a Macro

Let’s now look at some good use cases for each type of procedural macro. We’ll start with the easy one: derive macros.

## When to Use Derive Macros

Derive macros are for used one thing, and one thing only: to automate the implementation of a trait where automation is possible. Not all traits have obvious automated implementations, but many do. In practice, you should consider adding a derive macro for a trait only if the trait is implement often and if its implementation for any given type is fairly obvious. The first of these points may seem like common sense; if your trait is only going to be implemented once or twice, it’s probably not worth writing and maintaining a convoluted derive macro for it.

The second may seem stranger, however: what does it mean for the implementation to be “obvious”? Consider a trait like Debug. If you were told what Debug does and shown a type, you would probably expect an implementation of Debug to output the name of each field alongside the debug representation of its value. And that’s what derive(Debug) does. What about Clone? You’d probably expect it to just clone every field—and again, that’s what derive(Clone) does. With derive(serde::Serialize), we expect it to serialize every field and its value, and it does just that. In general, you want the derivation of a trait to match the developer’s intuition for what it probably does. If there is no obvious derivation for a trait, or worse yet, if your derivation does not match the obvious implementation, then you’re probably better off not giving it a derive macro.

## When to Use Function-Like Macros

Function-like procedural macros are harder to give a general rule of thumb for. You might say you should use function-like macros when you want a function-like macro but can’t express it with macro_rules!, but that’s a fairly subjective guideline. You can do a lot with declarative macros if you really put your mind to it, after all!

There are two particularly good reasons to reach for a function-like procedural macro:

- If you already have a declarative macro, and its definition is becoming so hairy that the macro is hard to maintain.
- If you have a pure function that you need to be able to execute at compile time, but cannot express with const fn. An example of this is the phf crate, which generates a hash map or set using a perfect hash function when given a set of keys provided at compile time. Another is hex-literal, which takes a string of hexadecimal characters and replaces it with the corresponding bytes. In general, anything that does not merely transform the input at compile time but actually computes over it is likely to be a good candidate.

I will also add that I do not recommend reaching for a function-like procedural macro just so that you can break hygiene within your macro. Hygiene for function-like macros is a feature that avoids many debugging headaches, and you should think very carefully before you intentionally break it.

## When to Use Attribute Macros

That leaves us with attribute macros. Though these are arguably the most general of procedural macros, it’s also hardest to pin down when they are appropriate. Over the years, I have seen four use cases come up time and time again as examples of attribute procedural macros adding tremendous value:

### Test generation

It is very common to want to run the same test under multiple different configurations, or many similar tests with the same bootstrapping code. While a declarative macro may let you express this, your code is often easier to read and maintain if you have an attribute like #[foo_test] that introduces a setup prelude and postscript in each annotated test, or a repeatable attribute like #[test_case(1)] #[test_case(2)] to mark that a given test should be repeated multiple times, once with each input.

### Framework annotations

Libraries like rocket use attribute macros to augment functions and types with additional information that the framework then makes use of without the user having to do a lot of manual configuration. It’s so much more convenient to be able to write #[get("/<name>")] fn

hello(name: String) than to have to set up a configuration struct with function pointers and the like. Essentially, the attributes make up a miniature domain-specific language (DSL) that hides a lot of boilerplate that’d otherwise be necessary. Similarly, the asynchronous I/O framework tokio lets you use #[tokio::main] async fn main() to automatically set up a runtime and run your asynchronous code, and thereby saves you from writing the same runtime setup in every asynchronous application’s main function.

### Transparent middleware

Some libraries want to inject themselves into your application in unobtrusive ways to provide added value that does not change the application’s functionality. For example, tracing and logging libraries like tracing and metric collection libraries like metered allow you to transparently instrument a function by adding an attribute to it, and then every call to that function will run some additional code dictated by the library.

### Type transformers

Sometimes you want to go beyond merely deriving traits for a type, and actually change the type’s definition in some fundamental way. In these cases, attribute macros are the way to go. The pin_project crate is a great example of this: its primary purpose is not to implement a particular trait, but rather to ensure that all pinned access to fields of a given type happens according to the strict rules that are set forth by Rust’s Pin type and the Unpin trait (we’ll talk more about those types in Chapter 9). It does this by generating additional helper types, adding methods to the annotated type, and introducing static safety checks to ensure that users don’t accidentally shoot themselves in the foot. While pin_project could have been implemented with a procedural derive macro, that would likely violate the expectation that a derived trait implementation is obvious.

## How Do They Work?

At the heart of all procedural macros is the TokenStream type, which can be iterated over to get the individual TokenTree items that make up that token stream. A TokenTree is either a single token—like an identifier, punctuation, or a literal—or another TokenStream enclosed in a delimiter like () or {}. By walking a TokenStream, you can parse out whatever syntax you wish as long as the individual tokens are valid Rust tokens. If you specifically want to parse your input as Rust code you likely want to use the syn crate, which implements a complete Rust parser and can turn a TokenStream into an easy-to-traverse Rust AST.

With most procedural macros you not only want to parse a TokenStream, but also to produce Rust code to be injected into the program that invokes the procedural macro. There are two main ways to do so. The first is to manually construct a TokenStream and extend it one TokenTree at a time. The second
